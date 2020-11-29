## CI for the godot engine inside a local gitlab instance

### How to launch gitlab with runner

On mac:  
`GITLAB_HOME=$HOME/gitlab docker-compose up`  
On linux:  
`GITLAB_HOME=/srv/gitlab docker-compose up`

Then you need to register a runner:  

Go to http://127.0.0.1/admin/runners, copy the token to register the runner. For example: DtXUwfkX5B7Pc_4eVzYP.

Run in console:
`docker exec -i gitlab_runner_1 gitlab-runner register --non-interactive --url http://gitlab_web_1 --registration-token <TOKEN> --docker-network-mode gitlab_default --executor docker --docker-image alpine:latest`

* `gitlab_runner_1` and `gitlab_web_1` are docker image names. To check them, run `docker ps` before.
* `--docker-network-mode gitlab_default` is the solution of the "network problem".
* `--executor docker --docker-image alpine:latest` are default docker images, if not set in `.gitlab-ci.yaml`.

## How to build godot on ci

1. Create a project on the local gitlab with the name Godot and user root.  
2. Push godot to the local gitlab:  
`cd godot`  
`git remote add gitlab http://127.0.0.1/root/godot.git`  
`git push gitlab`  
3. Open gitlab piplines: http://127.0.0.1/root/godot/-/pipelines

Godot with ci file is added as a submodule from gitlab: https://gitlab.com/pavelperc/godot  
Gitlab ci is located here: https://gitlab.com/pavelperc/godot/-/blob/master/.gitlab-ci.yml  

