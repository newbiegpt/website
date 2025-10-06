# Deployment Guide for Lolipop! Hosting

This project targets the Lolipop! shared hosting environment at `gonna-haruuuu0413.ssl-lolipop.jp`. Because the agent running in this workspace cannot reach external networks, deployment must be performed from your local machine using the credentials supplied in the Lolipop! control panel.

## 1. Enable and Verify SSH/SFTP Access
1. Sign in to the Lolipop! user page.
2. Navigate to **サーバーの管理・設定 → アカウント情報**.
3. Confirm that SSH is enabled and note the following connection details:
   - **Host**: `ssh.lolipop.jp`
   - **Port**: `2222`
   - **User**: `gonna.jp-haruuuu0413`
   - **Password**: Use the auto-generated password shown in the control panel (the value changes if you regenerate it). Keep it private and do not commit it to Git.

## 2. Prepare Your Local Environment
- Install an SFTP/SSH client such as FileZilla, Cyberduck, WinSCP, or use the command line (`sftp`, `scp`, or `rsync`).
- Ensure the project files (`index.html`, images, and any assets) are present on your machine.

## 3. Upload via GUI Client (Example: FileZilla)
1. Open FileZilla and create a new site entry.
2. Set **Protocol** to `SFTP`.
3. Enter the host, port, user, and password from above.
4. Connect and browse to the public directory (often `public_html` or a subdirectory created for the subdomain).
5. Drag and drop the site files from your local project folder into the remote directory. Allow overwriting if you are updating existing files.

## 4. Upload via Command Line
### Using `sftp`
```bash
sftp -P 2222 gonna.jp-haruuuu0413@ssh.lolipop.jp
# After connecting:
cd public_html
put index.html
put YY.png
put YYDES.png
put YYDEFullsize.png
bye
```
Adjust the `cd` path if your subdomain points to a different directory. You can also use `mput` to upload multiple files at once.

### Using `scp`
```bash
scp -P 2222 index.html YY*.png gonna.jp-haruuuu0413@ssh.lolipop.jp:public_html/
```
Replace `public_html/` with the correct remote path.

### Using `rsync`
```bash
rsync -avz -e "ssh -p 2222" ./ gonna.jp-haruuuu0413@ssh.lolipop.jp:public_html/
```
This command synchronizes the entire current directory with the remote directory while preserving timestamps.

## 5. Verify the Deployment
1. Visit `https://gonna-haruuuu0413.ssl-lolipop.jp/` in a browser.
2. If updates do not appear immediately, clear your browser cache or perform a hard reload (Ctrl+Shift+R / Cmd+Shift+R).
3. Confirm that images, scripts, and styles load correctly.

## 6. Security Best Practices
- Rotate the SSH password periodically from the Lolipop! control panel.
- Never commit credentials or `.ppk` files to version control.
- When working from shared computers, avoid saving credentials in FTP clients.
- Keep a local backup of the deployed files before making significant updates.

By following these steps you can deploy the site to Lolipop! hosting using the credentials you control while keeping sensitive information secure.
