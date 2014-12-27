title: Ext2文件系统源代码分析(0)
tags:
  - Ext2
id: 261
categories:
  - 编程相关
date: 2008-04-29 22:22:26
---

**Makefile:**

	#
	# Makefile for the linux ext2-filesystem routines.
	#

	obj-$(CONFIG_EXT2_FS) += ext2.o

	ext2-y := balloc.o dir.o file.o fsync.o ialloc.o inode.o \
	          ioctl.o namei.o super.o symlink.o

	ext2-$(CONFIG_EXT2_FS_XATTR)     += xattr.o xattr_user.o xattr_trusted.o
	ext2-$(CONFIG_EXT2_FS_POSIX_ACL) += acl.o
	ext2-$(CONFIG_EXT2_FS_SECURITY)  += xattr_security.o
	ext2-$(CONFIG_EXT2_FS_XIP)       += xip.o


obj-$(CONFIG_EXT2_FS)指定编译生成的目标文件，CONFIG_EXT2_FS变量在配置内核时指定，有三个选项：y,m,x分别代表编译，以模块方式编译，不编译。

ext2-y指定了编译ext2.o时所需要同时编译的其它文件。

下面四个是ext2文件系统的一些扩展属性，也是在编译内核时配置是否使用。

**ext2.h:**
	#include <linux /fs.h>
	#include </linux><linux /ext2_fs.h>

	/*
	 * ext2 mount options
	 */
	struct ext2_mount_options {
	        unsigned long s_mount_opt;
	        uid_t s_resuid;
	        gid_t s_resgid;
	};
	//这里定义了一个ext2文件系统挂载时所使用的一些选项，uid_t，gid_t在i386上是unsigned int。
	//s_mount_opt：安装选项
	//s_resuid：可以使用保留块的用户id
	//s_resgid：可以使用保留块的用户组id

	/*
	 * second extended file system inode data in memory
	 */
	struct ext2_inode_info {
	        __le32  i_data[15];
	        __u32   i_flags;
	        __u32   i_faddr;
	        __u8    i_frag_no;
	        __u8    i_frag_size;
	        __u16   i_state;
	        __u32   i_file_acl;
	        __u32   i_dir_acl;
	        __u32   i_dtime;

	        /*
	         * i_block_group is the number of the block group which contains
	         * this file's inode.  Constant across the lifetime of the inode,
	         * it is ued for making block allocation decisions - we try to
	         * place a file's data blocks near its inode block, and new inodes
	         * near to their parent directory's inode.
	         */
	        __u32   i_block_group;

	        /* block reservation info */
	        struct ext2_block_alloc_info *i_block_alloc_info;

	        __u32   i_dir_start_lookup;
	#ifdef CONFIG_EXT2_FS_XATTR
	        /*
	         * Extended attributes can be read independently of the main file
	         * data. Taking i_mutex even when reading would cause contention
	         * between readers of EAs and writers of regular file data, so
	         * instead we synchronize on xattr_sem when reading or changing
	         * EAs.
	         */
	        struct rw_semaphore xattr_sem;
	#endif
	#ifdef CONFIG_EXT2_FS_POSIX_ACL
	        struct posix_acl        *i_acl;
	        struct posix_acl        *i_default_acl;
	#endif
	        rwlock_t i_meta_lock;

	        /*
	         * truncate_mutex is for serialising ext2_truncate() against
	         * ext2_getblock().  It also protects the internals of the inode's
	         * reservation data structures: ext2_reserve_window and
	         * ext2_reserve_window_node.
	         */
	        struct mutex truncate_mutex;
	        struct inode    vfs_inode;
	        struct list_head i_orphan;      /* unlinked but open inodes */
	};

	//ext2_inode_info这个结构体定义了inode在内存中存放的信息。
	//__le32  i_data[15];  __le32是带bitwise属性的unsigned int；数据块指针数组
	//__u32   i_flags;  __u32是unsigned int；文件标志
	//__u32   i_faddr;  fragment地址
	//__u8    i_frag_no;  fragment号
	//__u8    i_frag_size;  fragment大小
	//__u16   i_state;  
	//__u32   i_file_acl;  文件访问控制链表
	//__u32   i_dir_acl;  目录访问控制链表
	//__u32   i_dtime;  文件删除时间 
	//__u32   i_block_group;  inode所在组号
	//struct ext2_block_alloc_info *i_block_alloc_info;  block保留信息

	/*
	 * Inode dynamic state flags
	 */
	#define EXT2_STATE_NEW                  0x00000001 /* inode is newly created */

	/*
	 * Function prototypes
	 */

	/*
	 * Ok, these declarations are also in </linux><linux /kernel.h> but none of the
	 * ext2 source programs needs to include it so they are duplicated here.
	 */

	static inline struct ext2_inode_info *EXT2_I(struct inode *inode)
	{
	        return container_of(inode, struct ext2_inode_info, vfs_inode);
	}
	//EXT2_I函数利用到了内核中经常使用的一个宏:container_of，它的参数为inode指针，返回一个ext2_inode_info的指针，也就是inode所在结构体的地址。
	//container_of在linux-2.6.24/include/linux/kernel.h里定义为：
	//#define container_of(ptr, type, member) ({                      \
	       const typeof( ((type *)0)->member ) *__mptr = (ptr);    \
	       (type *)( (char *)__mptr - offsetof(type,member) );})

	//这个宏的原理其实很简单就是利用结构体中某个元素的地址减去该元素相对于结构体的偏移量，得到结构体的地址。

	//offsetof在linux-2.6.24/include/linux/stddef.h中定义为：
	//#undef offsetof
	//#ifdef __compiler_offsetof
	//#define offsetof(TYPE,MEMBER) __compiler_offsetof(TYPE,MEMBER)
	//#else
	//#define offsetof(TYPE, MEMBER) ((size_t) &((TYPE *)0)->MEMBER)
	//#endif

	//接下来这这些是定义在ext2其它文件中的函数的申明。
	/* balloc.c */
	extern int ext2_bg_has_super(struct super_block *sb, int group);
	extern unsigned long ext2_bg_num_gdb(struct super_block *sb, int group);
	extern ext2_fsblk_t ext2_new_block(struct inode *, unsigned long, int *);
	extern ext2_fsblk_t ext2_new_blocks(struct inode *, unsigned long,
	                                unsigned long *, int *);
	extern void ext2_free_blocks (struct inode *, unsigned long,
	                              unsigned long);
	extern unsigned long ext2_count_free_blocks (struct super_block *);
	extern unsigned long ext2_count_dirs (struct super_block *);
	extern void ext2_check_blocks_bitmap (struct super_block *);
	extern struct ext2_group_desc * ext2_get_group_desc(struct super_block * sb,
	                                                    unsigned int block_group,
	                                                    struct buffer_head ** bh);
	extern void ext2_discard_reservation (struct inode *);
	extern int ext2_should_retry_alloc(struct super_block *sb, int *retries);
	extern void ext2_init_block_alloc_info(struct inode *);
	extern void ext2_rsv_window_add(struct super_block *sb, struct ext2_reserve_window_node *rsv);

	/* dir.c */
	extern int ext2_add_link (struct dentry *, struct inode *);
	extern ino_t ext2_inode_by_name(struct inode *, struct dentry *);
	extern int ext2_make_empty(struct inode *, struct inode *);
	extern struct ext2_dir_entry_2 * ext2_find_entry (struct inode *,struct dentry *, struct page **);
	extern int ext2_delete_entry (struct ext2_dir_entry_2 *, struct page *);
	extern int ext2_empty_dir (struct inode *);
	extern struct ext2_dir_entry_2 * ext2_dotdot (struct inode *, struct page **);
	extern void ext2_set_link(struct inode *, struct ext2_dir_entry_2 *, struct page *, struct inode *);

	/* fsync.c */
	extern int ext2_sync_file (struct file *, struct dentry *, int);

	/* ialloc.c */
	extern struct inode * ext2_new_inode (struct inode *, int);
	extern void ext2_free_inode (struct inode *);
	extern unsigned long ext2_count_free_inodes (struct super_block *);
	extern void ext2_check_inodes_bitmap (struct super_block *);
	extern unsigned long ext2_count_free (struct buffer_head *, unsigned);

	/* inode.c */
	extern void ext2_read_inode (struct inode *);
	extern int ext2_write_inode (struct inode *, int);
	extern void ext2_put_inode (struct inode *);
	extern void ext2_delete_inode (struct inode *);
	extern int ext2_sync_inode (struct inode *);
	extern int ext2_get_block(struct inode *, sector_t, struct buffer_head *, int);
	extern void ext2_truncate (struct inode *);
	extern int ext2_setattr (struct dentry *, struct iattr *);
	extern void ext2_set_inode_flags(struct inode *inode);
	extern void ext2_get_inode_flags(struct ext2_inode_info *);
	int __ext2_write_begin(struct file *file, struct address_space *mapping,
	                loff_t pos, unsigned len, unsigned flags,
	                struct page **pagep, void **fsdata);

	/* ioctl.c */
	extern int ext2_ioctl (struct inode *, struct file *, unsigned int,
	                       unsigned long);
	extern long ext2_compat_ioctl(struct file *, unsigned int, unsigned long);

	/* namei.c */
	struct dentry *ext2_get_parent(struct dentry *child);

	/* super.c */
	extern void ext2_error (struct super_block *, const char *, const char *, ...)
	        __attribute__ ((format (printf, 3, 4)));
	extern void ext2_warning (struct super_block *, const char *, const char *, ...)
	        __attribute__ ((format (printf, 3, 4)));
	extern void ext2_update_dynamic_rev (struct super_block *sb);
	extern void ext2_write_super (struct super_block *);

	/*
	 * Inodes and files operations
	 */

	/* dir.c */
	extern const struct file_operations ext2_dir_operations;

	/* file.c */
	extern const struct inode_operations ext2_file_inode_operations;
	extern const struct file_operations ext2_file_operations;
	extern const struct file_operations ext2_xip_file_operations;

	/* inode.c */
	extern const struct address_space_operations ext2_aops;
	extern const struct address_space_operations ext2_aops_xip;
	extern const struct address_space_operations ext2_nobh_aops;

	/* namei.c */
	extern const struct inode_operations ext2_dir_inode_operations;
	extern const struct inode_operations ext2_special_inode_operations;

	/* symlink.c */
	extern const struct inode_operations ext2_fast_symlink_inode_operations;
	extern const struct inode_operations ext2_symlink_inode_operations;

	static inline ext2_fsblk_t
	ext2_group_first_block_no(struct super_block *sb, unsigned long group_no)
	{
	        return group_no * (ext2_fsblk_t)EXT2_BLOCKS_PER_GROUP(sb) +
	                le32_to_cpu(EXT2_SB(sb)->s_es->s_first_data_block);
	}
