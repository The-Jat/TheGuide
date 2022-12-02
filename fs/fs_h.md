
src/system/kernel/fs/Vnode.h\
```c++

struct vnode : fs_vnode, DoublyLinkedListLinkImpl<vnode> {
			struct vnode*		next;
			VMCache*			cache;
			struct fs_mount*	mount;
			struct vnode*		covered_by;
			struct vnode*		covers;
			struct advisory_locking* advisory_locking;
			struct file_descriptor* mandatory_locked_by;
			list_link			unused_link;
			ino_t				id;
			dev_t				device;
			int32				ref_count;

public:
	inline	bool				IsBusy() const;
	inline	void				SetBusy(bool busy);

	inline	bool				IsRemoved() const;
	inline	void				SetRemoved(bool removed);

	inline	bool				IsUnpublished() const;
	inline	void				SetUnpublished(bool unpublished);

	inline	bool				IsUnused() const;
	inline	void				SetUnused(bool unused);

	inline	bool				IsHot() const;
	inline	void				SetHot(bool hot);

	// setter requires sVnodeLock write-locked, getter is lockless
	inline	bool				IsCovered() const;
	inline	void				SetCovered(bool covered);

	// setter requires sVnodeLock write-locked, getter is lockless
	inline	bool				IsCovering() const;
	inline	void				SetCovering(bool covering);

	inline	uint32				Type() const;
	inline	void				SetType(uint32 type);

	inline	bool				Lock();
	inline	void				Unlock();

	static	void				StaticInit();

private:
	static	const uint32		kFlagsLocked		= 0x00000001;
	static	const uint32		kFlagsWaitingLocker	= 0x00000002;
	static	const uint32		kFlagsBusy			= 0x00000004;
	static	const uint32		kFlagsRemoved		= 0x00000008;
	static	const uint32		kFlagsUnpublished	= 0x00000010;
	static	const uint32		kFlagsUnused		= 0x00000020;
	static	const uint32		kFlagsHot			= 0x00000040;
	static	const uint32		kFlagsCovered		= 0x00000080;
	static	const uint32		kFlagsCovering		= 0x00000100;
	static	const uint32		kFlagsType			= 0xfffff000;

	static	const uint32		kBucketCount		= 32;

			struct LockWaiter : DoublyLinkedListLinkImpl<LockWaiter> {
				LockWaiter*		next;
				Thread*			thread;
				struct vnode*	vnode;
			};

			typedef DoublyLinkedList<LockWaiter> LockWaiterList;

			struct Bucket {
				mutex			lock;
				LockWaiterList	waiters;

				Bucket();
			};

private:
	inline	Bucket&				_Bucket() const;

			void				_WaitForLock();
			void				_WakeUpLocker();

private:
			int32				fFlags;

	static	Bucket				sBuckets[kBucketCount];
};

'''
