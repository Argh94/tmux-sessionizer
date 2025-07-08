# tmux-sessionizer

**tmux-sessionizer** is a powerful, user-friendly Bash script for managing your [tmux](https://github.com/tmux/tmux) sessions based on project directories or predefined commands. It supports interactive selection (with [fzf](https://github.com/junegunn/fzf)), persistent split panes, custom session names, directory ignore functionality, and robust safety for executing project-level session scripts.

---

## Features | امکانات

- **Automatic tmux session creation and switching**  
  ایجاد خودکار session برای هر پروژه یا مسیر و جابجایی سریع بین آن‌ها  
- **Interactive directory/session selection** *(fzf support)*  
  انتخاب پروژه/سشن به صورت تعاملی با پیش‌نمایش (در صورت نصب fzf)  
- **Session commands**  
  اجرای دستورات سفارشی در هر سشن (در پنجره یا split)  
- **Persistent split pane management**  
  نگهداری splitهای ایجاد شده برای هر دستور/سشن  
- **Custom session naming** (`--name`)  
  انتخاب نام دلخواه برای session، با اعتبارسنجی  
- **Session name conflict management**  
  حل هوشمندانه تعارض نام سشن‌ها، با قابلیت جایگزینی سشن قبلی  
- **Directory/project ignore**  
  مخفی‌کردن پروژه‌ها/دایرکتوری‌ها با فایل `.tmux-sessionizer-ignore`  
- **Secure execution of .tmux-sessionizer scripts**  
  اجرای اسکریپت پروژه فقط با تایید کاربر یا اتوکانفرم  
- **Automatic fallback if tmux is not available**  
  اجرای bash در مسیر انتخابی اگر tmux نصب نباشد  
- **Logging**  
  ثبت لاگ عملیات (در فایل یا خروجی)  
- **Fully configurable**  
  پیکربندی کامل با فایل `~/.config/tmux-sessionizer/tmux-sessionizer.conf`

---

## Quick Start (English)

1. **Install tmux** and (optionally) [fzf](https://github.com/junegunn/fzf)
2. Copy `tmux-sessionizer` to a directory in your `$PATH` and make it executable:
   ```sh
   chmod +x tmux-sessionizer
   ```
3. *(Optional but recommended)* Create your config file:
   ```sh
   mkdir -p ~/.config/tmux-sessionizer
   cp tmux-sessionizer.conf.example ~/.config/tmux-sessionizer/tmux-sessionizer.conf
   # Or edit your own
   ```
4. **Run:**
   ```sh
   tmux-sessionizer
   ```
   - Interactively select a directory/session
   - Or directly:
     ```sh
     tmux-sessionizer ~/project/folder
     tmux-sessionizer --name mysession ~/project/folder
     tmux-sessionizer -s 0 --vsplit
     ```

## راه‌اندازی سریع (فارسی)

۱. **tmux** و (اختیاری) [fzf](https://github.com/junegunn/fzf) را نصب کنید  
۲. فایل اسکریپت را اجرایی کنید:
   ```sh
   chmod +x tmux-sessionizer
   ```
۳. (اختیاری) فایل تنظیمات شخصی بسازید:
   ```sh
   mkdir -p ~/.config/tmux-sessionizer
   # ویرایش فایل کانفیگ
   vim ~/.config/tmux-sessionizer/tmux-sessionizer.conf
   ```
۴. اجرا:
   ```sh
   tmux-sessionizer
   ```
   - یا اجرای مستقیم برای یک پروژه:
     ```sh
     tmux-sessionizer ~/project/folder
     tmux-sessionizer --name mysession ~/project/folder
     tmux-sessionizer -s 0 --vsplit
     ```

---

## Configuration

Edit `~/.config/tmux-sessionizer/tmux-sessionizer.conf`:

```bash
TS_SEARCH_PATHS=(~/projects ~/work)
TS_EXTRA_SEARCH_PATHS=(~/ghq:3)
TS_MAX_DEPTH=2
TS_SESSION_COMMANDS=("vim" "htop; top")
TS_LOG="file" # or "true" or "echo"
TS_AUTO_CONFIRM="true"
```

- Add `.tmux-sessionizer` in a project to run initialization code on session start (prompted for safety).
- Add `.tmux-sessionizer-ignore` in a folder to hide it from the project/session list.

---

## Usage (English)

```sh
tmux-sessionizer [OPTIONS] [PATH]
```

**Options:**
- `-h, --help`          Show help message
- `-s, --session <idx>` Run command at index in TS_SESSION_COMMANDS
- `--vsplit`            Vertical split (requires -s)
- `--hsplit`            Horizontal split (requires -s)
- `--no-tmux`           Open directory in bash if tmux is unavailable
- `--name <name>`       Use custom session name (validated)
- `-v, --version`       Show version

**Examples:**
- Open/switch session for a directory:  
  `tmux-sessionizer ~/code/myproject`
- Custom session name:  
  `tmux-sessionizer --name work ~/code/work`
- Run a session command in vsplit:  
  `tmux-sessionizer -s 1 --vsplit`
- Ignore a folder:  
  `touch ~/code/secret/.tmux-sessionizer-ignore`
- Auto-source session script:  
  `echo "export ENV=dev" > ~/code/myproject/.tmux-sessionizer`

---

## امکانات ویژه (فارسی)

- جستجو و لیست دایرکتوری‌ها با عمق دلخواه (TS_MAX_DEPTH)
- پشتیبانی از sessionهای موجود tmux و امکان جابجایی سریع بین آن‌ها
- پیش‌نمایش هوشمند پروژه و session در fzf (تعداد فایل، وجود اسکریپت، مسیر session)
- مدیریت cache پن‌های split و پاک‌سازی خودکار پن‌های مرده
- امکان حذف و جایگزینی session هم‌نام با تایید کاربر
- سازگاری کامل با bash و محیط‌های لینوکسی و macOS

---

## Security Notes

- Any `.tmux-sessionizer` file in a project directory will be sourced into the session pane upon creation **only after user confirmation** (or automatically if `TS_AUTO_CONFIRM=true`).
- If you see a session name conflict, you can choose to replace the existing tmux session.

---

## Troubleshooting

- If you see "tmux is not installed", either install tmux or use `--no-tmux`
- If fzf is not installed, fallback selection is used
- Ignore directories by creating `.tmux-sessionizer-ignore` file in them
- For advanced logging, set `TS_LOG="file"` and review `~/.local/share/tmux-sessionizer/tmux-sessionizer.log`

---

## License

MIT License

---

## Contributors

- [Argh94](https://github.com/Argh94)
- [Copilot](https://github.com/github/copilot)

---

**Enjoy fast, safe, and smart tmux session management!**

---

# tmux-sessionizer (فارسی)

**tmux-sessionizer** یک اسکریپت حرفه‌ای Bash برای مدیریت sessionهای tmux بر اساس پوشه پروژه یا دستورات سفارشی است.  
با انتخاب تعاملی، مدیریت split، نام‌گذاری دلخواه، پشتیبانی ignore و امنیت بالا، بهترین ابزار برای کاربران حرفه‌ای لینوکس و macOS است.

### امکانات کلیدی:
- ساخت و سوییچ session خودکار برای هر پروژه
- انتخاب حرفه‌ای مسیر/session (با fzf و پیش‌نمایش)
- اجرای اسکریپت ابتدای session (با تایید)
- امکان حذف session هم‌نام (با تایید)
- پشتیبانی از ignore پروژه/دایرکتوری
- انتخاب نام session دلخواه (و اعتبارسنجی)
- مدیریت cache پن‌ها و پاک‌سازی خودکار
- اجرای bash در مسیر انتخابی اگر tmux نصب نباشد
- لاگ‌گیری عملیات و پیکربندی کامل

### شروع سریع:
1. نصب tmux و (اختیاری) fzf
2. کپی و اجرایی‌کردن اسکریپت
3. ساخت فایل تنظیمات شخصی در `~/.config/tmux-sessionizer`
4. اجرای `tmux-sessionizer` و لذت بردن از سرعت و امنیت

### مستندات بیشتر و راهنمایی در help اسکریپت و کانفیگ مثال آورده شده است.

---
