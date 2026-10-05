# BÁO CÁO THỰC HÀNH KHÔI PHỤC COMMIT BẰNG GIT REFLOG

## 1. Mục tiêu

Thực hiện mô phỏng tình huống commit quan trọng bị mất sau khi sử dụng `git reset --hard HEAD~1`, sau đó sử dụng `git reflog` để tìm lại mã hash của commit và khôi phục commit về nhánh hiện tại.

## 2. Khởi tạo repository

Khởi tạo Git repository:

```bash
git init
```

Tạo file `README.md` và commit lần đầu:

```bash
git add .
git commit -m "Khoi tao bai tap"
```

## 3. Tạo commit quan trọng

Tạo file `feature.txt` với nội dung:

```text
Day la tinh nang quan trong
```

Sau đó thực hiện:

```bash
git add .
git commit -m "Them tinh nang quan trong"
```

Kiểm tra lịch sử:

```bash
git log --oneline
```

Kết quả hiển thị commit `Them tinh nang quan trong`.

## 4. Mô phỏng việc mất commit

Thực hiện lệnh:

```bash
git reset --hard HEAD~1
```

Lệnh này đưa `HEAD` và branch hiện tại trở về commit trước đó, đồng thời loại bỏ các thay đổi của commit cuối khỏi working directory.

Kiểm tra:

```bash
git log --oneline
```

Commit `Them tinh nang quan trong` không còn xuất hiện trong lịch sử hiện tại.

File `feature.txt` cũng không còn trong working directory.

## 5. Tìm lại commit bằng Reflog

Sử dụng:

```bash
git reflog
```

Kết quả cho thấy commit đã bị reset, ví dụ:

```text
e3a5b2c HEAD@{1}: commit: Them tinh nang quan trong
```

Trong đó `e3a5b2c` là mã hash của commit cần khôi phục.

## 6. Khôi phục commit

Sử dụng mã hash tìm được từ Reflog:

```bash
git reset --hard e3a5b2c
```

Sau khi thực hiện, commit được khôi phục trở lại branch hiện tại.

## 7. Kiểm tra kết quả

Kiểm tra lịch sử:

```bash
git log --oneline
```

Kết quả:

```text
e3a5b2c Them tinh nang quan trong
...
```

Kiểm tra file:

```bash
Get-Content feature.txt
```

Kết quả:

```text
Day la tinh nang quan trong
```

Kiểm tra trạng thái repository:

```bash
git status
```

Repository đã được khôi phục về trạng thái của commit quan trọng trước khi thực hiện `git reset --hard HEAD~1`.

## 8. Kết luận

Thông qua bài thực hành, em đã sử dụng `git reflog` để truy vết commit không còn xuất hiện trong `git log`. Sau khi xác định được mã hash của commit, em sử dụng `git reset --hard` với mã hash đó để đưa branch trở lại trạng thái trước khi commit bị mất.

Qua đó, có thể thấy `git reflog` là công cụ hữu ích để khôi phục các commit bị mất do thao tác như `reset`, miễn là commit vẫn còn được Git lưu giữ trong repository cục bộ.