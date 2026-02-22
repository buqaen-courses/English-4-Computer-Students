# انقلاب خط فرمان: از مبتدی GUI به Power User ترمینال

اولین برخورد خود را با خط فرمان همچون دیروز به یاد دارم. دانشجوی علوم کامپیوتر بودم، راحت با interfaces گرافیکی، وقتی professorم این prompt cryptic را روی صفحه drop کرد:

```
student@computer:~$
```

"به shell خوش آمدید" گفت. "اینجا جایی است که قدرت واقعی وجود دارد."

به آن cursor چشمک‌زن خیره شدم، احساس کردم به دنیای بیگانه‌ای منتقل شده‌ام. `ls`؟ `cd`؟ `grep`؟ هر کدام چه معنایی داشتند؟ اما با یادگیری navigate کردن در این interface text-based، چیزی magical اتفاق افتاد. کشف کردم که خط فرمان نه فقط relic از گذشته computing است - بلکه superpower بود که productivity من را incredibly افزایش داد.

خط فرمان پل بین intention انسانی و execution کامپیوتری است. جایی است که ایده‌ها به actions تبدیل می‌شوند، جایی که automation شروع می‌شود، و جایی که تفکر computational واقعی شکل می‌گیرد.

## چرا خط فرمان مهم است

در جهانی از interfaces drag-and-drop و اپلیکیشن‌های smartphone، چرا زحمت کشیدن با interface text-based که نیاز به memorize کردن دستورات arcane دارد؟

### قدرت Precision

GUIها برای exploration و وظایف بصری عالی هستند، اما complexity را پنهان می‌کنند. خط فرمان inner workings کامپیوتر شما را expose می‌کند، کنترل precise روی هر operation به شما می‌دهد.

### Automation و Efficiency

وقتی یاد بگیرید دستورات را combine کنید، می‌توانید وظایف پیچیده را با چند keystroke انجام دهید. آنچه در GUI ۱۰ کلیک ماوس نیاز دارد ممکن است در ترمینال یک دستور باشد.

### ضروری برای کار Remote

وقتی به سرورها SSH می‌کنید، زیرساخت cloud را مدیریت می‌کنید، یا با containers کار می‌کنید، GUI اغلب وجود ندارد. خط فرمان به universal interface شما تبدیل می‌شود.

### Superpower برنامه‌نویس

Workflowهای توسعه مدرن - Git، Docker، Kubernetes، CI/CD pipelines - همه heavily به ابزارهای خط فرمان متکی هستند. یادگیری CLI شما را به برنامه‌نویس effectiveتری تبدیل می‌کند.

## شروع: Navigation پایه

پیش از اینکه دستورات پیچیده را یاد بگیرید، بیایید اصول پایه را پوشش دهیم.

### درک ساختار فایل

**Absolute vs Relative Paths:**
- **Absolute:** از root شروع می‌شود (`/home/user/documents`)
- **Relative:** از directory فعلی شروع می‌شود (`documents/projects`)

**کارکردهای پایه:**
```bash
pwd          # نمایش directory فعلی
ls           # لیست محتویات directory
ls -la       # لیست مفصل با فایل‌های hidden
cd folder    # تغییر به directory
cd ..        # یک سطح بالا رفتن
cd ~         # رفتن به home directory
```

### مدیریت فایل‌ها

```bash
touch file.txt              # ایجاد فایل خالی
mkdir folder                # ایجاد directory
cp source destination       # کپی فایل
mv oldname newname          # تغییر نام یا انتقال
rm file                     # حذف فایل
rm -rf folder               # حذف directory و محتویات (خطرناک!)
```

### جستجو و فیلتر

```bash
grep "pattern" file         # جستجو برای pattern در فایل
grep -r "pattern" folder    # جستجو recursive در directory
find . -name "*.txt"        # یافتن فایل‌ها با الگو
cat file                    # نمایش محتویات فایل
head -10 file               # نمایش ۱۰ خط اول
tail -10 file               # نمایش ۱۰ خط آخر
```

## Redirection و Pipes: ترکیب دستورات

قدرت واقعی خط فرمان در ترکیب دستورات است.

### Redirection پایه

```bash
command > file              # خروجی را به فایل redirect کن (overwrite)
command >> file             # خروجی را به فایل append کن
command < file              # ورودی را از فایل بگیر
command 2> file             # خطاها را به فایل redirect کن
command > file 2>&1         # خروجی و خطاها را به فایل redirect کن
```

### Pipes: زنجیره‌سازی دستورات

```bash
command1 | command2         # خروجی command1 را به command2 feed کن
ls | grep "\.txt$"          # فایل‌های txt را پیدا کن
ps aux | grep python        # فرایندهای python را پیدا کن
cat file | wc -l            # تعداد خطوط فایل را بشمار
```

## Scripting: Automation واقعی

وقتی دستورات را در فایل ذخیره می‌کنید، automation واقعی شروع می‌شود.

### Bash Script پایه

```bash
#!/bin/bash

# این کامنت است
echo "Hello, World!"

# متغیرها
name="Alice"
echo "Hello, $name!"

# شرطی‌ها
if [ $1 = "test" ]; then
    echo "Running in test mode"
else
    echo "Running in production mode"
fi

# حلقه‌ها
for file in *.txt; do
    echo "Processing $file"
    wc -l "$file"
done
```

### Permissions و Execution

```bash
chmod +x script.sh          # script را executable کن
./script.sh                 # script را اجرا کن
./script.sh argument        # با argument اجرا کن
```

## ابزارهای قدرتمند

این ابزارها workflow شما را transform می‌کنند.

### Text Processing

```bash
sed 's/old/new/g' file      # جایگزینی متن
awk '{print $1}' file       # ستون‌ها را استخراج کن
sort file                   # مرتب‌سازی خطوط
uniq file                   # خطوط تکراری را حذف کن
cut -d',' -f1 file          # فیلدهای CSV را استخراج کن
```

### Network Tools

```bash
ping host                   # اتصال شبکه را تست کن
curl url                    # درخواست HTTP بفرست
wget url                    # فایل download کن
ssh user@host               # اتصال remote امن
scp file user@host:/path    # فایل را امن کپی کن
```

### Process Management

```bash
ps aux                      # فرایندهای running را نمایش بده
top                         # monitor فرایندها و منابع
kill PID                    # فرایند را متوقف کن
kill -9 PID                 # فرایند را force kill کن
nohup command &             # دستور را در background اجرا کن
```

## Package Management

ابزارهای مدیریت بسته بسته به سیستم عامل متفاوت هستند.

### Ubuntu/Debian (apt)

```bash
sudo apt update             # لیست بسته‌ها را update کن
sudo apt install package    # بسته نصب کن
sudo apt remove package     # بسته حذف کن
sudo apt search keyword     # بسته‌ها را جستجو کن
```

### CentOS/RHEL (yum/dnf)

```bash
sudo yum install package    # بسته نصب کن
sudo yum remove package     # بسته حذف کن
sudo yum search keyword     # بسته‌ها را جستجو کن
```

### macOS (brew)

```bash
brew install package        # بسته نصب کن
brew remove package         # بسته حذف کن
brew search keyword         # بسته‌ها را جستجو کن
```

## Git: کنترل نسخه ضروری

Git خط فرمان را به workflow توسعه مدرن تبدیل می‌کند.

### گردش کار پایه Git

```bash
git init                    # repository جدید initialize کن
git clone url               # repository را clone کن
git status                  # وضعیت را چک کن
git add file                # فایل را stage کن
git commit -m "message"     # تغییرات را commit کن
git push origin main        # به remote push کن
git pull origin main        # از remote pull کن
```

### Branching و Merging

```bash
git branch                  # branchها را لیست کن
git checkout -b new-branch  # branch جدید ایجاد کن
git merge branch            # branch را merge کن
git rebase main             # تاریخچه را clean کن
```

## Docker: Containerization

Docker اپلیکیشن‌ها را package و distribute می‌کند.

### دستورات پایه Docker

```bash
docker build -t image .     # image بساز
docker run image            # container اجرا کن
docker ps                   # containers running را لیست کن
docker stop container       # container متوقف کن
docker logs container       # logs را ببین
```

### Docker Compose برای Multi-Container

```yaml
# docker-compose.yml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: password
```

```bash
docker-compose up           # همه سرویس‌ها را راه‌اندازی کن
docker-compose down         # همه سرویس‌ها را متوقف کن
```

## SSH و کار Remote

SSH برای کار با سرورهای remote ضروری است.

### اتصال پایه

```bash
ssh user@hostname           # اتصال به سرور
ssh -i key.pem user@host    # با کلید SSH اتصال کن
ssh -L 8080:localhost:80 user@host  # port forwarding
```

### انتقال فایل‌ها

```bash
scp file user@host:/path    # فایل را کپی کن
rsync -av source/ user@host:/dest  # directory را sync کن
```

## Cron: زمان‌بندی وظایف

Cron وظایف را در زمان‌های مشخص اجرا می‌کند.

### Syntax Cron

```
* * * * * command
│ │ │ │ │
│ │ │ │ └─── روز هفته (0-7)
│ │ │ └───── ماه (1-12)
│ │ └─────── روز ماه (1-31)
│ └───────── ساعت (0-23)
└─────────── دقیقه (0-59)
```

### مثال‌های Cron

```bash
crontab -e                 # crontab را ویرایش کن
crontab -l                 # crontab را لیست کن

# هر روز ساعت ۲ صبح backup بگیر
0 2 * * * /home/user/backup.sh

# هر دوشنبه ساعت ۹ صبح گزارش اجرا کن
0 9 * * 1 /home/user/report.sh

# هر ۵ دقیقه چک کن
*/5 * * * * /home/user/check.sh
```

## امنیت در خط فرمان

خط فرمان می‌تواند خطرناک باشد اگر مراقب نباشید.

### بهترین روش‌ها

**هرگز دستورات را کپی-پیست نکنید بدون درک آنها**
```bash
# خطرناک - کل سیستم را حذف می‌کند!
rm -rf /*
```

**از sudo با احتیاط استفاده کنید**
```bash
# خوب
sudo apt update

# خطرناک - کل سیستم را با امتیازات root اجرا می‌کند
sudo ./untrusted-script.sh
```

**کلیدهای SSH را امن نگه دارید**
```bash
chmod 600 ~/.ssh/id_rsa    # permissions صحیح برای کلید خصوصی
```

## Scripting پیشرفته

با یادگیری بیشتر، scriptهای پیچیده‌تر بسازید.

### پردازش Arguments

```bash
#!/bin/bash

while [[ $# -gt 0 ]]; do
  case $1 in
    -h|--help)
      echo "Usage: $0 [-h] [-v] file"
      exit 0
      ;;
    -v|--verbose)
      verbose=true
      shift
      ;;
    *)
      file="$1"
      shift
      ;;
  esac
done

if [[ -n "$file" ]]; then
  echo "Processing $file"
  if [[ "$verbose" = true ]]; then
    echo "Verbose mode enabled"
  fi
fi
```

### مدیریت خطا

```bash
#!/bin/bash

set -e  # اگر دستوری شکست خورد، script متوقف شود

function cleanup() {
  echo "Cleaning up..."
  # cleanup code here
}

trap cleanup EXIT

# عملیات اصلی
echo "Starting operation..."
# اگر این شکست بخورد، cleanup اجرا می‌شود
risky_operation
echo "Operation completed successfully"
```

## ابزارهای مدرن

ابزارهای مدرن خط فرمان را قدرتمندتر می‌کنند.

### Fuzzy Finders

```bash
# fzf برای fuzzy finding
find . -name "*.txt" | fzf

# fd به جای find
fd pattern

# ripgrep به جای grep
rg pattern
```

### Terminal Multiplexers

```bash
# tmux برای sessionهای متعدد
tmux new -s session         # session جدید
tmux attach -t session      # attach به session
tmux detach                 # detach از session

# داخل tmux
Ctrl-b %                    # split vertical
Ctrl-b "                    # split horizontal
Ctrl-b arrow                # pane تغییر کن
```

## عیب‌یابی مشکلات رایج

مشکلات رایج و راه‌حل‌های آنها.

### Permission Denied

```bash
# فایل executable نیست
chmod +x script.sh
./script.sh

# دسترسی به directory ندارید
ls -la /path/to/directory
sudo ls /path/to/directory
```

### Command Not Found

```bash
# بسته نصب نیست
which command              # چک کن آیا نصب است
sudo apt install command   # نصب کن

# PATH تنظیم نیست
echo $PATH
export PATH=$PATH:/new/path
```

### Disk Full

```bash
df -h                       # فضای دیسک را چک کن
du -sh *                    # اندازه directoryها را چک کن
find . -size +100M          # فایل‌های بزرگ پیدا کن
```

## ساخت workflow شخصی شما

خط فرمان درباره یادگیری دستورات نیست - بلکه درباره ساخت سیستم‌هایی است که برای شما کار می‌کنند.

### Aliases برای Efficiency

```bash
# در ~/.bashrc یا ~/.zshrc اضافه کن
alias ll='ls -alF'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias update='sudo apt update && sudo apt upgrade'
```

### توابع مفید

```bash
# backup سریع
backup() {
  tar -czf "backup-$(date +%Y%m%d-%H%M%S).tar.gz" "$1"
}

# پیدا کردن و جایگزینی در فایل‌ها
find_replace() {
  find . -name "*.txt" -exec sed -i "s/$1/$2/g" {} \;
}
```

## نتیجه‌گیری: تفکر خط فرمان

خط فرمان درباره تایپ سریع نیست - بلکه درباره تفکر متفاوت است. درباره شکستن مشکلات به گام‌های کوچک، ترکیب ابزارها، و ساخت automation است.

هر برنامه‌نویس پیشرفته‌ای زمانی مبتدی بود. تفاوت کسانی که موفق می‌شوند این است که به یادگیری ادامه می‌دهند، experiment می‌کنند، و workflow خود را customize می‌کنند.

خط فرمان superpower شما است. با آن، می‌توانید:
- وظایف تکراری را automate کنید
- سرورها را از راه دور مدیریت کنید
- workflowهای توسعه پیچیده بسازید
- مشکلات را سریع عیب‌یابی کنید

شروع کنید. با دستورات پایه experiment کنید. scriptهای کوچک بنویسید. به تدریج، خواهید دید که نمی‌توانید بدون آن کار کنید.

خوش آمدید به دنیای خط فرمان. قدرت واقعی در نوک انگشتان شماست.