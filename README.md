# Synapse Vault

Bản đồ tri thức kiểu Obsidian chạy hoàn toàn trong trình duyệt: một file `index.html`, không cần build, không có backend.

## Chạy

Mở `index.html` bằng trình duyệt, hoặc chạy một server tĩnh bất kỳ:

```bash
python -m http.server 8000
```

rồi vào http://localhost:8000.

Trang có sẵn một vault mẫu (dữ liệu minh hoạ, có nhãn "Dữ liệu mẫu") để xem ngay.

## Tính năng

**Graph**
- Đồ thị force-directed vẽ trên canvas (d3-force + d3-zoom). Kích thước nút theo số liên kết, hub có quầng sáng và nhãn dạng viên thuốc.
- Tô màu theo **thư mục**, hoặc theo **cụm ý tưởng** (phát hiện cộng đồng bằng thuật toán Louvain). Mỗi cụm được kéo về một vùng riêng.
- Rê chuột hoặc chọn một nút: các nút lân cận sáng lên, có hạt chạy dọc liên kết theo chiều liên kết.
- Liên kết `[[...]]` tới ghi chú chưa tồn tại hiện thành nút rỗng nét đứt.
- Graph cục bộ (độ sâu 1 đến 3), bảng chỉnh lực và hiển thị, tìm kiếm có bỏ dấu tiếng Việt.

**Phân tích**
- **Đường nối hai ý tưởng**: đường ngắn nhất giữa hai ghi chú, có tuỳ chọn không đi tắt qua ghi chú Daily.
- **Phát lại**: xem graph lớn dần theo ngày tạo ghi chú.
- **Cầu nối giữa các cụm**: xếp hạng theo betweenness centrality (thuật toán Brandes).
- **Nhịp ghi chép**: heatmap 26 tuần; bấm một ô để làm sáng các ghi chú tạo hôm đó.
- **Cần chăm sóc**: ghi chú mồ côi và liên kết tới ghi chú chưa tạo.

## Dùng với dữ liệu của bạn

- **Vault Obsidian**: bấm "Mở vault của bạn" hoặc kéo thả thư mục vault vào graph. Trang đọc các file `.md`, lấy `[[wikilink]]`, link markdown tới `.md`, `#tag`, `tags` và `created`/`date` trong frontmatter. Thư mục cấp 1 trở thành nhóm màu. Các thư mục `.obsidian`, `.trash`, `.git` được bỏ qua.
- **JSON**: nhập file dạng

  ```json
  {
    "name": "ten-vault",
    "nodes": [{ "id": "A", "title": "Ghi chú A", "group": "Notes", "created": "2026-07-01", "tags": ["ai"] }],
    "links": [{ "source": "A", "target": "B" }]
  }
  ```

  Nút có `"ghost": true` được vẽ như ghi chú chưa tạo. Nút "Sao chép JSON" xuất graph hiện tại theo đúng định dạng này.

Mọi xử lý diễn ra trong trình duyệt; vault không được tải lên đâu cả.

## Phím tắt

| Phím | Tác dụng |
| --- | --- |
| `/` | Tìm ghi chú |
| `Esc` | Bỏ chọn / xoá đường / tắt graph cục bộ |
| `L` | Bật tắt graph cục bộ |
| `F` | Vừa khung |
| `Space` | Phát lại |
| Nhấp đúp vào nút | Mở graph cục bộ quanh nút đó |

## Công nghệ

- [d3](https://d3js.org) 7.9.0 nạp từ cdnjs
- Font Be Vietnam Pro và IBM Plex Mono từ Google Fonts
