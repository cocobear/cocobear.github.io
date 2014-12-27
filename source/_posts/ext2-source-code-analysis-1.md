title: Ext2文件系统源代码分析(1)
tags:
  - Ext2
id: 268
categories:
  - 编程相关
date: 2008-05-05 22:03:31
---

**acl.h:**

	/*
	  File: fs/ext2/acl.h

	  (C) 2001 Andreas Gruenbacher, <a .gruenbacher@computer.org>
	*/

	#include <linux /posix_acl_xattr.h>

	//简单说一下acl(访问控制表)，比传统的Linux具有更灵活的文件权限设定，ext2文件系统的扩展属性支持acl，这一点在编译的时候可以指定。
	#define EXT2_ACL_VERSION        0x0001

	//定义acl的版本；
	typedef struct {
	        __le16          e_tag;
	        __le16          e_perm;
	        __le32          e_id;
	} ext2_acl_entry;
	//__le16类型的定义：
	//typedef __u16 __bitwise __le16;
	//看起来比较复杂，其实（在x86平台下）只是一个unsigned short，__bitwise这个只是通过gcc的扩展使用sparse这个工具来进行代码检查。

	//ext2_acl_entry定义访问控制项的结构体
	//__le16          e_tag;   自身的使用者或組
	//__le16          e_perm;  允许访问的
	//__le32          e_id; 

	typedef struct {
	        __le16          e_tag;
	        __le16          e_perm;
	} ext2_acl_entry_short;
	//定义访问控制项的简单结构；

	typedef struct {
	        __le32          a_version;
	} ext2_acl_header;

	//接下来两个函数分别用到了size_t与ssize_t对应在x86平台下分别是unsigned int与int
	static inline size_t ext2_acl_size(int count)
	{
	        if (count < = 4) {
	                return sizeof(ext2_acl_header) +
	                       count * sizeof(ext2_acl_entry_short);
	        } else {
	                return sizeof(ext2_acl_header) +
	                       4 * sizeof(ext2_acl_entry_short) +
	                       (count - 4) * sizeof(ext2_acl_entry);
	        }
	}

	//该函数用来计算acl的占用空间大小，当使用的acl多于4后剩余的acl使用完整的ext2_acl_entry来记录。
	static inline int ext2_acl_count(size_t size)
	{
	        ssize_t s;
	        size -= sizeof(ext2_acl_header);
	        s = size - 4 * sizeof(ext2_acl_entry_short);
	        if (s < 0) {
	                if (size % sizeof(ext2_acl_entry_short))
	                        return -1;
	                return size / sizeof(ext2_acl_entry_short);
	        } else {
	                if (s % sizeof(ext2_acl_entry))
	                        return -1;
	                return s / sizeof(ext2_acl_entry) + 4;
	        }
	}
	//该函数与上面的函数刚好相反
	#ifdef CONFIG_EXT2_FS_POSIX_ACL

	//当内核编译时指定使用ext2的acl属性时这里ext2_permission等函数被申明，否则这些函数被定义为空。
	/* Value for inode->u.ext2_i.i_acl and inode->u.ext2_i.i_default_acl
	   if the ACL has not been cached */
	#define EXT2_ACL_NOT_CACHED ((void *)-1)

	/* acl.c */
	extern int ext2_permission (struct inode *, int, struct nameidata *);
	extern int ext2_acl_chmod (struct inode *);
	extern int ext2_init_acl (struct inode *, struct inode *);

	#else
	#include </linux><linux /sched.h>
	#define ext2_permission NULL
	#define ext2_get_acl    NULL
	#define ext2_set_acl    NULL

	static inline int
	ext2_acl_chmod (struct inode *inode)
	{
	        return 0;
	}

	static inline int ext2_init_acl (struct inode *inode, struct inode *dir)
	{
	        return 0;
	}
	#endif


**file.c:**

	/*
	 *  linux/fs/ext2/file.c
	 *
	 * Copyright (C) 1992, 1993, 1994, 1995
	 * Remy Card (card@masi.ibp.fr)
	 * Laboratoire MASI - Institut Blaise Pascal
	 * Universite Pierre et Marie Curie (Paris VI)
	 *
	 *  from
	 *
	 *  linux/fs/minix/file.c
	 *
	 *  Copyright (C) 1991, 1992  Linus Torvalds
	 *
	 *  ext2 fs regular file handling primitives
	 *
	 *  64-bit file support on 64-bit platforms by Jakub Jelinek
	 *      (jj@sunsite.ms.mff.cuni.cz)
	 */

	#include </linux><linux /time.h>
	#include "ext2.h"
	#include "xattr.h"
	#include "acl.h"

	/*
	 * Called when filp is released. This happens when all file descriptors
	 * for a single struct file are closed. Note that different open() calls
	 * for the same file yield different struct file structures.
	 */
	static int ext2_release_file (struct inode * inode, struct file * filp)
	{
	        if (filp->f_mode & FMODE_WRITE) {
	                mutex_lock(&EXT2_I(inode)->truncate_mutex);
	                ext2_discard_reservation(inode);
	                mutex_unlock(&EXT2_I(inode)->truncate_mutex);
	        }
	        return 0;
	}

	//该函数释放文件所使用的block，前提是该文件以"写"的模式被打开。
	/*
	 * We have mostly NULL's here: the current defaults are ok for
	 * the ext2 filesystem.
	 */
	const struct file_operations ext2_file_operations = {
	        .llseek         = generic_file_llseek,
	        .read           = do_sync_read,
	        .write          = do_sync_write,
	        .aio_read       = generic_file_aio_read,
	        .aio_write      = generic_file_aio_write,
	        .ioctl          = ext2_ioctl,
	#ifdef CONFIG_COMPAT
	        .compat_ioctl   = ext2_compat_ioctl,
	#endif
	        .mmap           = generic_file_mmap,
	        .open           = generic_file_open,
	        .release        = ext2_release_file,
	        .fsync          = ext2_sync_file,
	        .splice_read    = generic_file_splice_read,
	        .splice_write   = generic_file_splice_write,
	};

	//ext2_file_operations这个结构体定义了上层（VFS）文件操作函数在ext2文件系统这一层中的具体实现函数。
	//这里又用到了gcc的扩展使用.[index]这样的方式来初始化结构体，这样结构体中的成员就不会受到顺序的限制。
	#ifdef CONFIG_EXT2_FS_XIP
	const struct file_operations ext2_xip_file_operations = {
	        .llseek         = generic_file_llseek,
	        .read           = xip_file_read,
	        .write          = xip_file_write,
	        .ioctl          = ext2_ioctl,
	#ifdef CONFIG_COMPAT
	        .compat_ioctl   = ext2_compat_ioctl,
	#endif
	        .mmap           = xip_file_mmap,
	        .open           = generic_file_open,
	        .release        = ext2_release_file,
	        .fsync          = ext2_sync_file,
	};
	#endif

	//ext2_xip_file_operations定义了使用xip(eXecute In Place)的文件操作函数
	const struct inode_operations ext2_file_inode_operations = {
	        .truncate       = ext2_truncate,
	#ifdef CONFIG_EXT2_FS_XATTR
	        .setxattr       = generic_setxattr,
	        .getxattr       = generic_getxattr,
	        .listxattr      = ext2_listxattr,
	        .removexattr    = generic_removexattr,
	#endif
	        .setattr        = ext2_setattr,
	        .permission     = ext2_permission,
	};
	//ext2_file_inode_operations定义了ext2中inode的文件操作函数。
