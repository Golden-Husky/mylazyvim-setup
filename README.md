# 💤 mylazyvim-setup

ชุดตั้งค่า Neovim ที่ใช้ LazyVim พร้อมธีม ปุ่มลัด และปลั๊กอินสำหรับเขียนโค้ด

สำหรับ Ubuntu หรือ Ubuntu ใน WSL บนเครื่อง Intel/AMD 64-bit (`x86_64`) รันคำสั่งด้วยบัญชีผู้ใช้ปกติ โดยใช้ `sudo` สำหรับติดตั้งเครื่องมือ ส่วนการโคลนและเปิด Neovim ไม่ต้องใช้ `sudo`

## 1. ลงเครื่องมือพื้นฐาน

```bash
sudo apt update
sudo apt install -y \
  git curl ca-certificates build-essential \
  tar gzip unzip ripgrep fd-find \
  xsel wl-clipboard nodejs npm \
  python3 python3-venv python3-pip
```

## 2. ลง Neovim

```bash
sudo apt update
sudo apt install -y neovim
nvim --version
```

> ชุดปลั๊กอินใน lockfile นี้ควรใช้ Neovim 0.12 ขึ้นไป หาก `apt` ได้รุ่นเก่ากว่านี้ ให้ใช้ [วิธีติดตั้ง Neovim ทางการ](https://github.com/neovim/neovim/blob/master/INSTALL.md#linux) ก่อนทำต่อ

## 3. ลง Tree-sitter CLI

รันทีละบรรทัด หากมี error ให้หยุดแก้ก่อนทำต่อ:

```bash
curl -fL --retry 3 https://github.com/tree-sitter/tree-sitter/releases/download/v0.26.1/tree-sitter-linux-x64.gz -o /tmp/tree-sitter.gz
gzip -dc /tmp/tree-sitter.gz > /tmp/tree-sitter
sudo install -m 755 /tmp/tree-sitter /usr/local/bin/tree-sitter
tree-sitter --version
```

ถ้าขึ้น `tree-sitter 0.26.1` ก็ติดตั้งเรียบร้อย

## 4. โคลนแล้วเปิดใช้งาน

ถ้ามี `~/.config/nvim` อยู่แล้ว ให้สำรองก่อน

```bash
mkdir -p ~/.config
git clone https://github.com/Golden-Husky/mylazyvim-setup.git ~/.config/nvim
nvim
```

รอปลั๊กอินติดตั้งเสร็จ แล้วปิด–เปิด Neovim อีกครั้ง

> **Clipboard:** config เดิมใช้ `xsel` สำหรับ X11 บน Wayland/WSL อาจต้องปรับ `lua/config/options.lua` ส่วน Docker ที่ไม่มี display อาจคัดลอก–วางข้ามโปรแกรมไม่ได้ แต่ยังทดสอบตัว editor ได้

## 5. ตรวจการติดตั้ง

กด `Esc` แล้วพิมพ์คำสั่งใน Neovim ตามด้วย `Enter`:

| คำสั่ง | ใช้ดูอะไร |
| --- | --- |
| `:Lazy` | รายการปลั๊กอิน สถานะติดตั้ง และข้อผิดพลาด |
| `:Mason` | เครื่องมือภาษา เช่น language server และ formatter |
| `:LazyHealth` | ตรวจสุขภาพชุด LazyVim |
| `:checkhealth` | ตรวจ dependencies และ provider ของ Neovim |
| `:LazyExtras` | เลือกส่วนเสริมที่ต้องการใช้ |

ลองเปิดไฟล์ภาษาที่ใช้จริง เช่น `.c` หรือ `.py` ตรวจสีโค้ดและ completion สำหรับ clipboard บน desktop ให้พิมพ์ข้อความ กด `Esc` ตามด้วย `"+yy` แล้วไปวางในโปรแกรมอื่น ข้อความควรตรงกัน หากขึ้น `Can't open display` ให้ตรวจการเชื่อมต่อ clipboard แทนการติดตั้งปลั๊กอินใหม่ทั้งหมด

เครื่องมือเฉพาะภาษายังต้องเตรียมตามงาน เช่น JDK สำหรับ Java หรือ CMake สำหรับโปรเจกต์ CMake ปลั๊กอินไม่ใช่ตัว compiler/runtime หาก Mason ติดตั้งไม่สำเร็จ ให้อ่าน error ของแพ็กเกจนั้น รวมถึงข้อกำหนดเวอร์ชัน Node.js/Python แทนการถือว่า `apt` ให้รุ่นที่ใหม่พอเสมอ

## ปรับให้เป็นของตัวเอง

- แก้ `user` และ `mail` ใน `lua/plugins/42header.lua` ก่อนใช้ `F1` เพื่อไม่สร้าง header ด้วยข้อมูลของเจ้าของ repo เดิม
- ปรับตัวเลือกทั่วไปใน `lua/config/options.lua` และปุ่มลัดใน `lua/config/keymaps.lua`
- เพิ่มหรือปรับปลั๊กอินใน `lua/plugins/`
- ถ้าไอคอนเป็นสี่เหลี่ยม ให้ติดตั้ง Nerd Font และเลือกใช้ใน terminal ที่แสดง Neovim; สำหรับ WSL มักเป็นการตั้งค่าฟอนต์ฝั่ง Windows
