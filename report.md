# Báo cáo Thực hành - Bài 1: Khởi tạo Workflow chạy Unit Test tự động cho Java Spring Boot với Gradle

## 1. Mục tiêu bài lab
- Khởi tạo tệp kịch bản **GitHub Actions Workflow** cho dự án Java Spring Boot sử dụng công cụ quản lý build **Gradle**.
- Thiết lập môi trường **Java JDK 17 (Temurin)** trên runner GitHub Actions.
- Tự động hóa quá trình cấp quyền thực thi và chạy toàn bộ bài kiểm thử JUnit Test bằng Gradle Wrapper (`./gradlew test`) mỗi khi có sự kiện `push` lên nhánh `main`.

---

## 2. Cấu trúc thư mục dự án
```text
Lesson1/
├── .github/
│   └── workflows/
│       └── ci.yml               # File cấu hình GitHub Actions Workflow
├── src/
│   ├── main/java/com/example/demo/
│   │   └── DemoApplication.java # Lớp khởi chạy Spring Boot
│   └── test/java/com/example/demo/
│       └── DemoApplicationTests.java # Lớp kiểm thử JUnit 5
├── build.gradle                 # Cấu hình Gradle dependencies và plugins
├── settings.gradle              # Cấu hình tên project
├── gradlew                      # Gradle wrapper script (Linux/macOS)
├── gradlew.bat                  # Gradle wrapper script (Windows)
└── report.md                    # Báo cáo thực hiện bài lab
```

---

## 3. Nội dung tệp cấu hình `.github/workflows/ci.yml`

```yaml
name: Java Spring Boot CI (Gradle)

on:
  push:
    branches: [ "main" ]

jobs:
  test:
    name: Run Unit Tests
    runs-on: ubuntu-latest

    steps:
      - name: Checkout mã nguồn
        uses: actions/checkout@v5

      - name: Thiết lập môi trường Java JDK 17
        uses: actions/setup-java@v5
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Cấp quyền cho Gradle Wrapper
        run: chmod +x gradlew

      - name: Thực thi JUnit Test với Gradle
        run: ./gradlew test
```

### Giải thích các thành phần trong Workflow:
1. **`name: Java Spring Boot CI (Gradle)`**: Tên của workflow, hiển thị trên giao diện Actions của GitHub.
2. **`on.push.branches: [ "main" ]`**: Bộ kích hoạt (Trigger) - Workflow sẽ tự động chạy mỗi khi có commit mới được push lên nhánh `main`.
3. **`runs-on: ubuntu-latest`**: Chạy job kiểm thử trên môi trường máy ảo Ubuntu phiên bản mới nhất do GitHub cung cấp.
4. **`actions/checkout@v5`**: Tải toàn bộ mã nguồn của repository về môi trường máy ảo của runner.
5. **`actions/setup-java@v5`**: Cài đặt JDK 17 với bản phân phối **Eclipse Temurin** (Adoptium), đảm bảo đồng nhất môi trường runtime với dự án.
6. **`run: chmod +x gradlew`**: Gán quyền thực thi (executable) cho tệp shell script `gradlew` trên hệ điều hành Linux.
7. **`run: ./gradlew test`**: Kích hoạt Gradle wrapper để biên dịch mã nguồn test và chạy toàn bộ JUnit tests. Nếu có bất kỳ test nào thất bại, job sẽ báo lỗi (Fail/Đỏ), nếu tất cả test vượt qua, job sẽ báo thành công (Success/Xanh).

---

## 4. Các bước triển khai và kiểm tra

### Bước 1: Khởi tạo Git và Commit mã nguồn
Mở terminal tại thư mục dự án và thực hiện:
```bash
git init
git add .
git commit -m "feat: setup spring boot gradle project and ci workflow"
```

### Bước 2: Liên kết với GitHub Remote Repository và Push
```bash
git branch -M main
git remote add origin <URL_REPOSITORY_CUA_BAN>
git push -u origin main
```

### Bước 3: Kiểm tra kết quả trên GitHub Actions
1. Truy cập vào GitHub Repository của bạn trên trình duyệt.
2. Nhấp vào tab **Actions**.
3. Bạn sẽ thấy workflow **Java Spring Boot CI (Gradle)** được kích hoạt tự động với commit vừa push.
4. Nhấp vào lần chạy (run) đó, chọn Job **Run Unit Tests**:
   - Bước **Checkout mã nguồn**: Hoàn thành.
   - Bước **Thiết lập môi trường Java JDK 17**: Hoàn thành.
   - Bước **Cấp quyền cho Gradle Wrapper**: Hoàn thành.
   - Bước **Thực thi JUnit Test với Gradle**: Hoàn thành (`BUILD SUCCESSFUL`).
5. Kết quả hiển thị dấu tích xanh lá (**Success**).

---

## 5. Sản phẩm nộp bài
- **Tệp kịch bản:** `.github/workflows/ci.yml`
- **Đường dẫn nộp bài gợi ý:** `homework/session_11/ex1/`
- **Minh chứng:** Ảnh chụp màn hình tab **Actions** trên GitHub hiển thị Job **Run Unit Tests** có trạng thái tích xanh (**Success**).
