# Linux Shell and Command Line Study Notes

This note mainly talks about commonly used command lines and shell scripts, which will be maintained and updated on a regular basis.

## tmux

`tmux` is a session management tool which enables putting tasks in backend and keeps them running.

### Create a New Session

```sh
tmux new -s new_session_name
```

The will create a new session and enter it. If you want to temporarily leave the session (or put it in the backend), `CTRL + B` and then press `D`.

### List all Sessions

```sh
tmux ls
```

### Switch Back to Session

```sh
tmux a -t session_name
```


### Kill a Session

```sh
tmux kill-session -t session_name
```


