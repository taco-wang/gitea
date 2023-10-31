# how to devolpe gitea

## how to start gitea

you should have make installed. then run the command below.
```bash
go mod tidy 
TAGS="bindata sqlite sqlite_unlock_notify" make build
```
after this, you will get a executable file named `gitea` in the root directory of gitea project. then you can start gitea by run `./gitea` in the root directory of gitea project.

## how to debug gitea

open or create a file named `.launch.json` placed in `.vscode` directory of root directory . then add the config above
```json
   {
      "name": "Launch (with SQLite3)",
      "type": "go",
      "request": "launch",
      "mode": "debug",
      "buildFlags": "-tags='bindata sqlite sqlite_unlock_notify'",
      "program": "${workspaceRoot}/main.go",
      "env": {
        "GITEA_WORK_DIR": "${workspaceRoot}",
      },
      "args": [
        "web"
      ],
      "showLog": true
    }
```
after this, you can start the progress by click the debug button in vscode.

## how to change the table structure 

it's not very clear in the doc of gitea, I guess it and it works. Maybe it's not the best way to do this, but it works.

1. change the table structure in `models` directory.
2. then create a new folder in `models/migrations` directory, like the old file named `v1_21/v276.go`.
   call the func, `x.sync()`

   ```go
    type Review struct {
      // default 1 not a approval , 2 is a approval
      ApprovalType int `xorm:"NOT NULL DEFAULT 1"`
    }
    if err := x.Sync(new(Mirror), new(PushMirror), new(Review)); err != nil {
      return err
    }
    ```

3. then run `./gitea migrate`.