title: Ext2文件系统源代码分析(2)
tags:
  - Ext2
id: 269
categories:
  - 编程相关
date: 2008-05-05 22:41:13
---

**xattr.h**:

	/*
	  File: linux/ext2_xattr.h

	  On-disk format of extended attributes for the ext2 filesystem.

	  (C) 2001 Andreas Gruenbacher, <a .gruenbacher@computer.org>
	*/

	#include <linux /init.h>
	#include </linux><linux /xattr.h>

	/* Magic value in attribute blocks */
	#define EXT2_XATTR_MAGIC                0xEA020000
	//定义属性block中使用的魔数(一个标记，唯一的值)

	/* Maximum number of references to one attribute block */
	#define EXT2_XATTR_REFCOUNT_MAX         1024
	//定义一个属性block

	/* Name indexes */
	#define EXT2_XATTR_INDEX_USER                   1
	#define EXT2_XATTR_INDEX_POSIX_ACL_ACCESS       2
	#define EXT2_XATTR_INDEX_POSIX_ACL_DEFAULT      3
	#define EXT2_XATTR_INDEX_TRUSTED                4
	#define EXT2_XATTR_INDEX_LUSTRE                 5
	#define EXT2_XATTR_INDEX_SECURITY               6

	struct ext2_xattr_header {
	        __le32  h_magic;        /* magic number for identification */
	        __le32  h_refcount;     /* reference count */
	        __le32  h_blocks;       /* number of disk blocks used */
	        __le32  h_hash;         /* hash value of all attributes */
	        __u32   h_reserved[4];  /* zero right now */
	};
	//__le32 h_magic; 标识码，为0xEA020000 

	//__le32 h_refcount; 属性块被链接的数目 

	//__le32 h_blocks; 用于扩展属性的块数 

	//__le32 h_hash;  所有属性的哈希值 

	//__u32 h_reserved[4]; 保留变量，目前为0

	struct ext2_xattr_entry {
	        __u8    e_name_len;     /* length of name */
	        __u8    e_name_index;   /* attribute name index */
	        __le16  e_value_offs;   /* offset in disk block of value */
	        __le32  e_value_block;  /* disk block attribute is stored on (n/i) */
	        __le32  e_value_size;   /* size of attribute value */
	        __le32  e_hash;         /* hash value of name and value */
	        char    e_name[0];      /* attribute name */
	};

	//__u8 e_name_len; 属性名长度 

	//__u8 e_name_index; 属性名索引 

	//__le16 e_value_offs; 属性值在值块中的偏移量 

	//__le32 e_value_block;  保存属性值的块的块号 

	//__le32 e_value_size;  属性值长度 

	//__le32 e_hash;  属性名和值的哈希值 

	//char e_name[0];  属性名

	#define EXT2_XATTR_PAD_BITS             2
	#define EXT2_XATTR_PAD          (1< <EXT2_XATTR_PAD_BITS)
	#define EXT2_XATTR_ROUND                (EXT2_XATTR_PAD-1)
	#define EXT2_XATTR_LEN(name_len) \
	        (((name_len) + EXT2_XATTR_ROUND + \
	        sizeof(struct ext2_xattr_entry)) & ~EXT2_XATTR_ROUND)
	#define EXT2_XATTR_NEXT(entry) \
	        ( (struct ext2_xattr_entry *)( \
	          (char *)(entry) + EXT2_XATTR_LEN((entry)->e_name_len)) )
	#define EXT2_XATTR_SIZE(size) \
	        (((size) + EXT2_XATTR_ROUND) & ~EXT2_XATTR_ROUND)

	# ifdef CONFIG_EXT2_FS_XATTR
	//当内核编译时指定使用xattr扩展属性时，xattr相关函数的申明

	extern struct xattr_handler ext2_xattr_user_handler;
	extern struct xattr_handler ext2_xattr_trusted_handler;
	extern struct xattr_handler ext2_xattr_acl_access_handler;
	extern struct xattr_handler ext2_xattr_acl_default_handler;
	extern struct xattr_handler ext2_xattr_security_handler;

	extern ssize_t ext2_listxattr(struct dentry *, char *, size_t);

	extern int ext2_xattr_get(struct inode *, int, const char *, void *, size_t);
	extern int ext2_xattr_set(struct inode *, int, const char *, const void *, size_t, int);

	extern void ext2_xattr_delete_inode(struct inode *);
	extern void ext2_xattr_put_super(struct super_block *);

	extern int init_ext2_xattr(void);
	extern void exit_ext2_xattr(void);

	extern struct xattr_handler *ext2_xattr_handlers[];

	# else  /* CONFIG_EXT2_FS_XATTR */

	//当内核编译时未使用xattr扩展属性，xattr相关函数被置为空

	static inline int
	ext2_xattr_get(struct inode *inode, int name_index,
	               const char *name, void *buffer, size_t size)
	{
	        return -EOPNOTSUPP;
	}

	static inline int
	ext2_xattr_set(struct inode *inode, int name_index, const char *name,
	               const void *value, size_t size, int flags)
	{
	        return -EOPNOTSUPP;
	}

	static inline void
	ext2_xattr_delete_inode(struct inode *inode)
	{
	}

	static inline void
	ext2_xattr_put_super(struct super_block *sb)
	{
	}

	static inline int
	init_ext2_xattr(void)
	{
	        return 0;
	}

	static inline void
	exit_ext2_xattr(void)
	{
	}

	#define ext2_xattr_handlers NULL

	# endif  /* CONFIG_EXT2_FS_XATTR */

	#ifdef CONFIG_EXT2_FS_SECURITY
	extern int ext2_init_security(struct inode *inode, struct inode *dir);
	#else
	static inline int ext2_init_security(struct inode *inode, struct inode *dir)
	{
	        return 0;
	}
	#endif
	[/c]

	**xip.h**:
	[c]
	/*
	 *  linux/fs/ext2/xip.h
	 *
	 * Copyright (C) 2005 IBM Corporation
	 * Author: Carsten Otte (cotte@de.ibm.com)
	 */

	#ifdef CONFIG_EXT2_FS_XIP
	//当内核编译时指定使用xip扩展属性时，xip相关操作的函数申明
	extern void ext2_xip_verify_sb (struct super_block *);
	extern int ext2_clear_xip_target (struct inode *, int);

	static inline int ext2_use_xip (struct super_block *sb)
	{
	        struct ext2_sb_info *sbi = EXT2_SB(sb);
	        return (sbi->s_mount_opt & EXT2_MOUNT_XIP);
	}
	struct page* ext2_get_xip_page (struct address_space *, sector_t, int);
	#define mapping_is_xip(map) unlikely(map->a_ops->get_xip_page)
	#else
	//当内核编译时未指定使用xattr扩展属性时，置这些函数操作为空

	#define mapping_is_xip(map)                     0
	#define ext2_xip_verify_sb(sb)                  do { } while (0)
	#define ext2_use_xip(sb)                        0
	#define ext2_clear_xip_target(inode, chain)     0
	#define ext2_get_xip_page                       NULL
	#endif

## Comments

**[cocobear](#3148 "2008-05-06 07:47:10"):** 你也找一个好做得呗……

**[luguo](#3147 "2008-05-05 23:31:16"):** 哎，dp同学的毕设好做啊，写写注释就完～！ ;-)

