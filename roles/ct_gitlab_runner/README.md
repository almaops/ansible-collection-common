almaops.ct_gitlab_runner
=========

This role deploys gitlab-runner in docker container, and register it in gitlab.

Role Variables
--------------
`ct_gitlab_runner_name`: the name that will be seen on runners page, default is `"{{ inventory_hostname }}"`

`ct_gitlab_runner_ct_name`: container name, default is `"gitlab-runner"`, change it if you're going to run multiple runners on the same host

`ct_gitlab_runner_registration_token`: this token is needed to register, can be acquired from project for specific runners, and from admin panel for shared runners; mandatory parameter, no default

`ct_gitlab_runner_coordinator_url`: address of your gitlab, by default is configured to `"https://gitlab.com/ci"`; change it if you have an on-premise gitlab installation

`ct_gitlab_runner_tags`: subj; default is `[]`

`ct_gitlab_runner_executor`: subj; default is `"docker"` as it's the most convenient way; no other executors are tested

`ct_gitlab_runner_parallel_builds_number`: the default is `"1"`

`ct_gitlab_runner_run_untagged`: specify if runner has to run untagged jobs, default is `"true"`

`ct_gitlab_runner_docker_image`: the default is `"alpine"`

`ct_gitlab_runner_path_srv_dir`: base dir to determine where to store service configs, default is `"/srv"`

`ct_gitlab_runner_path_cfg_dir`: final dir, where service configs are stored, default is `"{{ ct_gitlab_runner_path_srv_dir }}/{{ ct_gitlab_runner_ct_name }}/config"`

`ct_gitlab_runner_path_cfg`: runner config file, default is `"{{ ct_gitlab_runner_path_cfg_dir }}/config.toml"`; do not change it, it's really `config.toml`, I swear

`ct_gitlab_runner_ct_image_tag`: runner version, i.e. gitlab-runner's image tag, default is `"alpine"`

`ct_gitlab_runner_ct_image`: image of gitlab-runner container, default is `"gitlab/gitlab-runner:{{ ct_gitlab_runner_ct_image_tag }}"`; change only if you want to have some custom build of gitlab-runner container

`ct_gitlab_runner_ct_restart_policy`: container restart policy, default is `"always"`

`ct_gitlab_runner_ct_state`: container state after deploy, default is `"started"`

`ct_gitlab_runner_ct_restart`: if we need to restart container after deploy, default is `"false"`

`ct_gitlab_runner_ct_pull`: if we need to try to pull the image while deploy, default is `"false"`

`ct_gitlab_runner_ct_recreate`: if we need to recreate the container, default is `"false"`

`ct_gitlab_runner_cache_s3_enabled`: if we're going to use S3/Minio for caches, default is `false`

`ct_gitlab_runner_cache_s3_server_address`: subj, mandatory if s3 enabled, no default

`ct_gitlab_runner_cache_s3_access_key`: subj, mandatory if s3 enabled, no default

`ct_gitlab_runner_cache_s3_secret_key`: subj, mandatory if s3 enabled, no default

`ct_gitlab_runner_force_reg`: if we want to re-register this runner; default is `false`

`ct_gitlab_runner_docker_volumes`: additional mounts for containers runned by docker executor, default is `[]`; useful if you want to pass `docker.sock` or to mount `/cache` volume

`ct_gitlab_runner_dind_enabled`: set to `true` if you need your docker containers to run in priveleged mode; default is `false`

`ct_gitlab_runner_ct_volumes`: by default just mounts a configuration directory into runner container, i.e. `[ "{{ ct_gitlab_runner_path_cfg_dir }}:/etc/gitlab-runner:rw" ]`

`ct_gitlab_runner_dond_enabled`: if set to `true` (and this is the default), we also pass `/var/run/docker.sock` into runner container

There're several more interesting variables. Have a look on them: [defaults/main.yml](defaults/main.yml)

Example Playbook
----------------

    - hosts:
        - gitlab_runners
      become: true
      vars:
	    # common cfg
	    ct_gitlab_runner_ct_image_tag: "v14.8.1"
        ct_gitlab_runner_cache_s3_enabled: true
        ct_gitlab_runner_cache_s3_server_address: "{{ minio_bind_ip }}:{{ minio_bind_port }}"
        ct_gitlab_runner_cache_s3_access_key: "{{ minio_access_key }}"
        ct_gitlab_runner_cache_s3_secret_key: "{{ minio_secret_key }}"
		ct_gitlab_runner_name: "{{ runner_name }}_{{ inventory_hostname }}"
		gitlab_runner_ct_name: "gitlab-runner_{{ runner_name }}"
		# tokens
		main_registration_token: "<...>"
		proj_registration_token: "<...>"
      roles:
        - role: almaops.ct_gitlab_runner
		  tags:
		    - runner-tag-common
		  vars:
		    runner_name: "common"
		    ct_gitlab_runner_registration_token: "{{ main_registration_token }}"
		    ct_gitlab_runner_parallel_builds_number: "3"
			ct_gitlab_runner_tags:
			  - common
        - role: almaops.ct_gitlab_runner
		  tags:
		    - runner-tag-proj
		  vars:
		    runner_name: "proj"
		    ct_gitlab_runner_registration_token: "{{ proj_registration_token }}"
		    ct_gitlab_runner_parallel_builds_number: "1"
			ct_gitlab_runner_run_untagged: "false"
			ct_gitlab_runner_tags:
			  - proj


License
-------
MIT

Author Information
------------------
Dmitrii Kashin, <freehck@freehck.com>
