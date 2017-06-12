# Constructors

Classes are overused. More testable is simple function. Use classes
only when you need to keep state between two function calls.

## Constructor 

-  **must** setup *all* attributes representing class' state
-  **must not** setup attributes not important for class state
-  **must not** do any action besides setting attributes
-  **should** construct private attributes from the public ones

### Why?

Because of testability. For every class you are going to test you
need to know in what state it is. If constructor was running some
functions in background you are not the one in control. That will
lead to potentially different class state with the same construct
parameters.

### I need runtime information because it defines state of class

No you do not. Runtime information should be obtained in runtime
by a function/method. Class state never depends on external runtime
states.

### How do I distinguish class-state and runtime attribute?

The rule of thumb for me is: *If you have used an attribute 
only once (or in one method) then it is a runtime information*.

When you put runtime information into attributes you will make
your app hard to extend and messy.


## Example

    class Computer:
        def __init__(self, ncpu, ram):
            # simple asignment is ok
            self.ram = ram

            # construct private attribute from a public one
            # I prefer to save the public one too in case of serialization/deserialization
            self.ncpu = ncpu
            self._cpu_list = [CPU(n) for n in range(ncpu)]

            # this should never happen
            # do NOT perform any actions in constructor
            self.constraintRAM(ram)


### When you put all runtime information into attributes

    class Computer:

        def __init__(self,
                     software_root,
                     instance_root,
                     master_url,
                     computer_id,
                     buildout,
                     logger,
                     maximum_periodicity=86400,
                     key_file=None,
                     cert_file=None,
                     signature_private_key_file=None,
                     signature_certificate_list=None,
                     download_binary_cache_url=None,
                     upload_binary_cache_url=None,
                     download_from_binary_cache_url_blacklist=None,
                     upload_to_binary_cache_url_blacklist=None,
                     upload_cache_url=None,
                     download_binary_dir_url=None,
                     upload_binary_dir_url=None,
                     upload_dir_url=None,
                     master_ca_file=None,
                     certificate_repository_path=None,
                     promise_timeout=3,
                     shacache_ca_file=None,
                     shacache_cert_file=None,
                     shacache_key_file=None,
                     shadir_ca_file=None,
                     shadir_cert_file=None,
                     shadir_key_file=None,
                     forbid_supervisord_automatic_launch=False,
                     develop=False,
                     software_release_filter_list=None,
                     computer_partition_filter_list=None,
                     software_min_free_space=None,
                     instance_min_free_space=None,
                     instance_storage_home=None,
                     ipv4_global_network=None,
                     firewall_conf={},
                     ):

