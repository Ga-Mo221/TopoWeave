<div align="center">

# TopoWeave

**Vẽ nét lên mesh, nhận lại lưới quad sạch: retopology ngay trong viewport Blender.**

[![Blender](https://img.shields.io/badge/Blender-4.2%2B-E87D0D?logo=blender&logoColor=white)](https://www.blender.org/)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![C++](https://img.shields.io/badge/L%C3%B5i-C%2B%2B%20(t%C3%B9y%20ch%E1%BB%8Dn)-00599C?logo=cplusplus&logoColor=white)](#dựng-từ-mã-nguồn)
[![License](https://img.shields.io/badge/License-GPL--3.0--or--later-blue)](#giấy-phép)
[![Status](https://img.shields.io/badge/Tr%E1%BA%A1ng%20th%C3%A1i-d%C3%B9ng%20n%E1%BB%99i%20b%E1%BB%99-orange)](#trạng-thái-dự-án)

[Bắt đầu nhanh](#bắt-đầu-nhanh) · [Tính năng](#tính-năng) · [Phím tắt](#phím-tắt) · [Dựng từ mã nguồn](#dựng-từ-mã-nguồn) · [Changelog](CHANGELOG.md)

</div>

TopoWeave là add-on retopology cho Blender. Bạn vẽ các đường chính lên mesh gốc, tool tự tìm những vùng khép kín, dựng lưới xem trước cho từng vùng rồi Generate thành mesh retopo.

- **Vẽ như phác thảo, sửa như vector.** Nét bám bề mặt và tự nối thành một mạng node, cung, mặt. Cắt nhau là tạo giao điểm, kéo node là lưới cập nhật theo.
- **Số chia tự khớp giữa các vùng.** Đổi số chia một cạnh, các patch liên quan tự cân lại để toàn bộ lưới vẫn nối liền.
- **Dữ liệu không dễ mất.** Network được sao lưu theo vòng ra đĩa, ngoài file `.blend`, và khôi phục được từ Preferences.

> [!NOTE]
> Giao diện add-on hiện bằng tiếng Việt. Dự án đang ở giai đoạn dùng nội bộ, xem [Trạng thái dự án](#trạng-thái-dự-án).

## Bắt đầu nhanh

**Cần:** Blender 4.2 trở lên (đã thử trên 5.0.1).

1. Tải zip add-on từ [Releases](https://github.com/Ga-Mo221/TopoWeave/releases).
2. Trong Blender: **Edit → Preferences → Get Extensions → ⌄ → Install from Disk…**, chọn tệp zip.
3. Chọn mesh gốc, bấm nút **Sketch** trên header viewport (hoặc `Ctrl+Alt+S`) để bật thanh vẽ.

Rồi retopo vùng đầu tiên:

| Bước | Thao tác | Kết quả |
|---|---|---|
| 1 | `D`, kéo nét bao quanh một vùng | Vùng khép kín được nhận ra |
| 2 | `F`, click trong vùng | Hiện lưới xem trước |
| 3 | `Shift` + lăn trên cạnh | Đổi số chia, patch kề tự khớp |
| 4 | `T` | Đổi mẫu lưới của vùng |
| 5 | Click lần nữa | Generate vào mesh retopo |

```mermaid
flowchart LR
    A[Vẽ nét] --> B[Chiếu lên bề mặt]
    B --> C[Network<br/>node · cung · mặt]
    C --> D[Lưới xem trước]
    D --> E[Generate]
    E --> F[Chỉnh lưới]
    D -. sửa số chia, mẫu, cực .-> C
```

## Tính năng

| Nhóm | Bạn có gì |
|---|---|
| **Draw** | Vẽ tự do, Pen đặt neo (`Shift`), tẩy nét (`Ctrl`), snap vào nét có sẵn, đối xứng X / Y / Z |
| **Edit** | Chọn, kéo node, cọ làm mượt, proportional, gộp node, làm thẳng cung, đổi loại node |
| **Patch** | Lưới xem trước, mẫu lưới, Loop Insert, kéo cực, cọ mật độ, heatmap chất lượng |
| **Cylinder / Bridge** | Kéo một nét ngang qua tay, chân để tạo vòng quanh chi, chọn hai vòng để nối |
| **Chỉnh lưới** | Grab, Smooth, Relax và kéo đỉnh, cạnh, mặt với `G` / `R` / `S` trên lưới đã Generate |
| **Auto Retopo** | Tự dựng network từ QuadriFlow để bạn sửa tiếp bằng tay; Auto vùng dựng lại riêng một khu vực |
| **Duyệt lỗi** | Tô các chỗ cần sửa (mặt thiếu lưới, lưới méo, cực xấu) và nhảy lần lượt bằng `N` |
| **UV** | Đổi đường cắt UV thành nét, xem trước UV, chép UV và Seam từ mesh gốc khi Generate |
| **Bảng vẽ** | Bánh xe ảo: giữ `Y` rồi kéo bút để thay mọi thao tác lăn chuột |
| **An toàn dữ liệu** | Vòng sao lưu tự động, khôi phục được, nâng định dạng cũ; đọc được dữ liệu SketchRetopo cũ |

**Mẫu lưới** cho từng vùng: 4 góc, 4 góc tham số hóa, tâm, gom cạnh, O-grid, nửa O-grid, góc O-grid, chuyển tiếp, góc bo A / B và 1 tam giác. Bạn còn có thể lưu mẫu riêng.

## Phím tắt

Phím hoạt động khi thanh vẽ đang bật. Mọi phím đổi được ở **Preferences → Phím & chuột → Phím tắt**, và trùng phím sẽ được báo ngay.

<details open>
<summary><b>Chuyển công cụ và phím chung</b></summary>

| Phím | Thao tác |
|---|---|
| `D` | Draw (bấm nhanh: chuyển hẳn, giữ: dùng tạm) |
| `G` | Edit / Select |
| `F` | Patch / Generate |
| `B` | Cylinder / Bridge |
| `W` | Chỉnh lưới |
| `Y` | Bánh xe ảo cho bút |
| `N` / `Shift+N` | Duyệt lỗi: chỗ kế / chỗ trước |
| `H` / `Alt+H` | Lasso ẩn vùng mesh gốc / hiện lại tất cả |
| `Ctrl+Shift+R` | Chiếu lại network lên mesh gốc đã đổi |
| `Ctrl+Z` / `Ctrl+Shift+Z` | Undo / Redo của tool |

</details>

<details>
<summary><b>Patch</b></summary>

| Phím | Thao tác |
|---|---|
| Click | Lưới xem trước, click lần nữa để Generate |
| `Shift` + lăn trên cạnh | Đổi số chia |
| `Ctrl` + lăn trên cạnh | Đổi số chia, giữ nguyên hướng |
| `Ctrl` + click | Loop Insert (thêm `Shift`: chỉ trong một patch) |
| `T` | Đổi mẫu lưới (`1`–`9` chọn trong bảng mẫu) |
| `L` | Khóa số chia cung |
| `E` | Cân đều vòng |
| `R` / `Shift+R` | Trượt điểm chia / đặt lại chia đều |
| `V`, `[`, `]` | Cọ mật độ và bán kính cọ |
| `Q` / `Shift+Q` | Heatmap chất lượng / tới lưới xấu nhất |

</details>

<details>
<summary><b>Edit</b></summary>

| Phím | Thao tác |
|---|---|
| `C` | Xóa node giữ hình / thêm node trên cạnh |
| `X` | Xóa |
| `M` | Gộp node |
| `O` | Bật / tắt proportional |
| `V` | Đổi loại node (Free → Corner → Smooth) |
| `T` | Làm thẳng cung đang chọn |
| `-` / `=` | Giảm / tăng số chia cạnh |

</details>

<details>
<summary><b>Dùng với bút và bảng vẽ</b></summary>

Bút không có bánh xe, nên TopoWeave có **bánh xe ảo**: giữ `Y` rồi kéo bút lên / xuống, mỗi 20 px là một nấc lăn.

- Dùng được cho số chia cạnh, mật độ lưới, số chia toàn cục, bán kính cọ và bán kính proportional.
- Giữ thêm `Ctrl` để thành `Ctrl` + lăn.
- ExpressKey chỉ gửi nhấn + nhả thì chạm nhanh để khóa. Nhấc bút, chạm lại phím hoặc `Esc` để thoát.

</details>

## Sao lưu và khôi phục

TopoWeave lưu network, nét và lưới vào file `.blend`, đồng thời giữ một vòng bản sao ngoài file đó:

- Mặc định 20 bản, ghi 5 phút một lần khi có thay đổi. Mỗi lần lưu, mở file hoặc khôi phục cũng có thêm một bản.
- Bản hỏng bị từ chối trước khi đụng vào dữ liệu hiện tại.
- Khôi phục ở **Preferences → Chung → Ghi nét & thư mục → Sao lưu network**, hoặc F3 → *Khôi phục từ bản sao lưu*.

> [!IMPORTANT]
> Mesh đã Generate không nằm trong bản sao lưu. Đổi tên hoặc chuyển file `.blend` thì vòng cũ không tự hiện, hãy mở thư mục sao lưu từ Preferences.

## Dựng từ mã nguồn

| Thành phần | Yêu cầu |
|---|---|
| Python | 3.11 (cùng bản với Blender) |
| Phụ thuộc phát triển | `numpy`, `pytest`, `hypothesis`, `ruff` |
| Lõi C++ (tùy chọn) | CMake 3.26+, Visual Studio Build Tools 2022 trên Windows |

Đóng gói add-on:

```bash
py -3.11 scripts/make_dev_zip.py topoweave_addon.zip
```

Chạy các bước kiểm tra giống CI:

```bash
pip install numpy pytest ruff hypothesis
```

```bash
ruff check .
```

```bash
python scripts/check_core_boundary.py
```

```bash
pytest
```

Lõi C++ `sr_native` chỉ tăng tốc: không có nó, add-on tự dùng bản Python, chậm hơn nhưng cùng kết quả. Preferences cho biết bản nào đang chạy. Cách build nằm ở [native/README.md](native/README.md).

### Cấu trúc

```text
TopoWeave/
├── core/py/      lõi thuần Python, không import bpy: network, patch, persist, ui...
├── topoweave/    phần gắn với Blender: operator, vẽ GPU, Preferences
├── native/       lõi C++ sr_native (nanobind)
├── tests/        bộ kiểm thử pytest và hypothesis
├── bench/        đo hiệu năng
├── scripts/      đóng gói và kiểm tra
└── docs/         kế hoạch và thiết kế
```

`core/` không được import `bpy`, `gpu`, `bmesh` hay `mathutils`, và CI kiểm tra quy tắc này. Nhờ vậy phần lớn logic chạy và kiểm thử được ngoài Blender.

## Trạng thái dự án

TopoWeave đang ở **bản 0.1.0, dùng nội bộ**. Nên biết trước khi dùng:

| Hạng mục | Hiện tại |
|---|---|
| Blender | Khai báo từ 4.2; mới thử thực tế trên 5.0.1 |
| Lõi C++ dựng sẵn | Chỉ Windows x64, Python 3.11; macOS và Linux dùng bản Python |
| Ngôn ngữ giao diện | Tiếng Việt |
| Bút thật | Bánh xe ảo mới thử bằng sự kiện giả lập |

Tệp `.blend` cũ của SketchRetopo mở được và tự nâng định dạng. Chiều ngược lại thì không: SketchRetopo không đọc được dữ liệu đã lưu bằng TopoWeave.

## Giấy phép

Add-on khai báo [GPL-3.0-or-later](https://www.gnu.org/licenses/gpl-3.0.html) trong manifest.
