
```c++

typedef struct net_socket {
	struct net_protocol*	first_protocol;
	struct net_protocol_module_info* first_info;

	int						family;
	int						type;
	int						protocol;

	struct sockaddr_storage	address;
	struct sockaddr_storage	peer;

	int						options;
	int						linger;
	uint32					bound_to_device;

	struct {
		uint32		buffer_size;
		uint32		low_water_mark;
		bigtime_t	timeout;
	}						send, receive;

	status_t				error;
} net_socket;

```
