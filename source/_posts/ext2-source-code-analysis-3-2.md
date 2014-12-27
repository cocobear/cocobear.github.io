title: Ext2文件系统源代码分析(3)
tags:
  - Ext2
id: 271
categories:
  - 编程相关
date: 2008-05-06 09:58:17
---

**symlink.c:**

	/*
	 *  linux/fs/ext2/symlink.c
	 *
	 * Only fast symlinks left here - the rest is done by generic code. AV, 1999
	 *
	 * Copyright (C) 1992, 1993, 1994, 1995
	 * Remy Card (card@masi.ibp.fr)
	 * Laboratoire MASI - Institut Blaise Pascal
	 * Universite Pierre et Marie Curie (Paris VI)
	 *
	 *  from
	 *
	 *  linux/fs/minix/symlink.c
	 *
	 *  Copyright (C) 1991, 1992  Linus Torvalds
	 *
	 *  ext2 symlink handling code
	 */

	#include "ext2.h"
	#include "xattr.h"
	#include <linux /namei.h>

	static void *ext2_follow_link(struct dentry *dentry, struct nameidata *nd)
	{
	        struct ext2_inode_info *ei = EXT2_I(dentry->d_inode);
	        nd_set_link(nd, (char *)ei->i_data);
	        return NULL;
	}
	//ext2_follow_link()函数用于搜索符号连接所在的目标文件

	const struct inode_operations ext2_symlink_inode_operations = {
	        .readlink       = generic_readlink,
	        .follow_link    = page_follow_link_light,
	        .put_link       = page_put_link,
	#ifdef CONFIG_EXT2_FS_XATTR
	        .setxattr       = generic_setxattr,
	        .getxattr       = generic_getxattr,
	        .listxattr      = ext2_listxattr,
	        .removexattr    = generic_removexattr,
	#endif
	};
	//ext2_symlink_inode_operations定义了上层(VFS)符号连接操作在ext2文件系统这一层中的具体实现函数

	const struct inode_operations ext2_fast_symlink_inode_operations = {
	        .readlink       = generic_readlink,
	        .follow_link    = ext2_follow_link,
	#ifdef CONFIG_EXT2_FS_XATTR
	        .setxattr       = generic_setxattr,
	        .getxattr       = generic_getxattr,
	        .listxattr      = ext2_listxattr,
	        .removexattr    = generic_removexattr,
	#endif
	};
	//ext2_fast_symlink_inode_operations应该是为了向下兼容2.4内核使用的符号连接操作函数


**fsync.c:**

	/*
	 *  linux/fs/ext2/fsync.c
	 *
	 *  Copyright (C) 1993  Stephen Tweedie (sct@dcs.ed.ac.uk)
	 *  from
	 *  Copyright (C) 1992  Remy Card (card@masi.ibp.fr)
	 *                      Laboratoire MASI - Institut Blaise Pascal
	 *                      Universite Pierre et Marie Curie (Paris VI)
	 *  from
	 *  linux/fs/minix/truncate.c   Copyright (C) 1991, 1992  Linus Torvalds
	 * 
	 *  ext2fs fsync primitive
	 *
	 *  Big-endian to little-endian byte-swapping/bitmaps by
	 *        David S. Miller (davem@caip.rutgers.edu), 1995
	 * 
	 *  Removed unnecessary code duplication for little endian machines
	 *  and excessive __inline__s. 
	 *        Andi Kleen, 1997
	 *
	 * Major simplications and cleanup - we only need to do the metadata, because
	 * we can depend on generic_block_fdatasync() to sync the data blocks.
	 */

	#include "ext2.h"
	#include </linux><linux /buffer_head.h>          /* for sync_mapping_buffers() */

	/*
	 *      File may be NULL when we are called. Perhaps we shouldn't
	 *      even pass file to fsync ?
	 */

	int ext2_sync_file(struct file *file, struct dentry *dentry, int datasync)
	{
	        struct inode *inode = dentry->d_inode;
	        int err;
	        int ret;

	        ret = sync_mapping_buffers(inode->i_mapping);
	        if (!(inode->i_state & I_DIRTY))
	                return ret;
	        if (datasync && !(inode->i_state & I_DIRTY_DATASYNC))
	                return ret;

	        err = ext2_sync_inode(inode);
	        if (ret == 0)
	                ret = err;
	        return ret;
	}
	//ext2_sync_file函数对文件进行同步，它只检查inode中的脏数据，通过对ext2_sync_inode调用把数据写入硬盘
	//sync_mapping_buffers作用是写出地址空间所有间接的blocks

