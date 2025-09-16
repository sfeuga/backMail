# backMail

A simple Mail backup solution

## Requirements

- Ruby 3.4.5+
- any shell and terminal
- an email account to back up accessible via IMAP (+SSL)

## Install

### For macOS, GNU Linux and *BSD

1. Download and extract a release
2. From the extracted download folder, run `bundle install`
3. (Optional) Add `backMail` to your path
4. Done

### For Windows (soon)

1. Download, extract and install a release
2. Done

## Usage

From a terminal, run (change the path to `backMail` according to your situation):

`./backMail -e your@email.com -p 'your password' -s imap-server.email.com`

> Note: only `email`, `password` and `imap_server` are mandatory values.
>
> You can run `./backMail -h` to display the help, there is many more options to discover

You can set an account config file using a YAML file instead of using command lines options.
The file should be like:

```yaml
email: "your-email@example.com"
password: "your-password"
imap_server: "imap.example.com"
imap_port: 993
output_dir: "/path/to/output/dir"
```

> Note: Again, only `email`, `password` and `imap_server` are mandatory values.
>
> Security Note: there is a password in this files, so, pay attention where you upload it and to prevent leak, this file
> should be read/write (even better, this file can be read only) only for the user who run this app. You can achieve
> this under `macOS`, `GNU Linux` and `*BSD` by changing the permission like `chmod go-rwx my_config_file.yaml`.

### Advanced usage

If you want to back up multiple accounts, you can create multiple config file, one per account and create a cron task to
automate this process.

Example with this two accounts: `account_1@example.com` and `account_2@example.com`, assuming `backMail` is installed
and in your `PATH`.

1. Create two `YAML` config files:

    - One named `account_1.yaml`

        ```yaml
        email: "account_1@example.com"
        password: "super secured password"
        imap_server: "imap.example.com"
        ```

    - The other named `account_2.yaml`

        ```yaml
        email: "account_2@example.com"
        password: "another secured password"
        imap_server: "imap.example.com"
        ```

2. And create two [`cron`](https://crontab.guru/) task
    - Task for `account_1.yaml`, every day at 5 pm: `echo "0 17 * * * backMail -c ~/.config/account_1.yaml" | crontab -`
    - Task for `account_2.yaml`, every day, every 5 minutes, with logs output into a file:
      `echo "*/5 * * * * backMail -c ~/another/random/path/account_2.yaml >> ~/.local/share/backMail_account_2.log 2>&1" | crontab -`

## TODO

- Add a toggle to disable save of attachments
- Add shell completion (BASH, zsh, fish, powershell)
- Add a man page
- Add examples and documentation here 🤦‍♂️