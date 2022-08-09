### Following summarizes the field names used by Docker and Kubernetes.

| Description                         | Docker field name | Kubernetes field name |
|-------------------------------------|-------------------|-----------------------|
| The command run by the container    | Entrypoint        | command               |
| The arguments passed to the command | Cmd               | args                  |

When you override the default Entrypoint and Cmd, these rules apply:

- If you do not supply command or args for a Container, the defaults defined in the Docker image are used.
- If you supply a command but no args for a Container, only the supplied command is used. The default EntryPoint and the default Cmd defined in the Docker image are ignored.
- If you supply only args for a Container, the default Entrypoint defined in the Docker image is run with the args that you supplied.
- If you supply a command and args, the default Entrypoint and the default Cmd defined in the Docker image are ignored. Your command is run with your args.

Here are some examples:

| Image Entrypoint | Image Cmd   | Container command | Container args | Command run      |
|------------------|-------------|-------------------|----------------|------------------|
| `[/ep-1]`        | `[foo bar]` | `<not set>`       | `<not set>`    | `[ep-1 foo bar]` |
| `[/ep-1]`        | `[foo bar]` | `[/ep-2]`         | `<not set>`    | `[ep-2]`         |
| `[/ep-1]`        | `[foo bar]` | `<not set>`       | `[zoo boo]`    | `[ep-1 zoo boo]` |
| `[/ep-1]`        | `[foo bar]` | `[/ep-2]`         | `[zoo boo]`    | `[ep-2 zoo boo]` |
