# Facebook Comment Exporter

Userscript cào toàn bộ comment Facebook — bao gồm reply lồng nhau, emoji, sticker, @tag — và xuất ra file **Excel** hoặc **JSON**.

---

## Cài đặt

**Bước 1:** Cài [Tampermonkey](https://tampermonkey.net/) cho Chrome/Edge hoặc Firefox.

**Bước 2:** Nhấn nút bên dưới để cài script:

[![Install](https://img.shields.io/badge/Install-Facebook%20Comment%20Exporter-1877f2?style=for-the-badge)](https://github.com/Amorydev/Comments-Exporter-FB/raw/refs/heads/main/facebook-comment-scraper.user.js)

**Bước 3:** Tampermonkey sẽ hỏi xác nhận → nhấn **Install**.

---

## Cách dùng

1. Mở bất kỳ bài viết Facebook nào có comment
2. Một **panel nổi** sẽ xuất hiện góc trên phải — có thể **kéo di chuyển** tùy ý
3. Cấu hình tùy chọn:
   - **Max Comments**: giới hạn số comment (0 = không giới hạn)
   - **Định dạng**: Excel (.xlsx) hoặc JSON
   - **Fields**: chọn từng trường dữ liệu muốn xuất
4. Nhấn **Bắt đầu Scrape**
5. Script tự động:
   - Cuộn và load toàn bộ comment
   - Mở tất cả reply (kể cả reply lồng nhiều cấp)
   - Tô màu viền từng comment đã cào được
   - Hiển thị bộ đếm thời gian thực
6. Sau khi xong, file tự động tải về

---

## Dữ liệu thu thập được

| Field | Mô tả | Mặc định |
|-------|--------|----------|
| `author` | Tên người comment | ✅ |
| `text` | Nội dung comment (kể cả emoji, sticker) | ✅ (bắt buộc) |
| `timestamp` | Thời gian đăng | ✅ |
| `likes` | Số lượt thích | ✅ |
| `depth` | Cấp độ lồng nhau (0=main, 1=reply, 2=reply của reply...) | ✅ |
| `replyToAuthor` | Tên người được reply tới | ✅ |
| `profileUrl` | Link profile Facebook | ✅ |
| `tagName` | Tên người được @tag trong comment | ✅ |
| `isReply` | Có phải reply không | ☐ |
| `parentId` | ID comment cha | ☐ |
| `profileImage` | URL ảnh đại diện | ☐ |
| `isMedia` | Có phải sticker/GIF/ảnh không | ☐ |
| `stickerUrl` | URL của sticker/GIF | ☐ |
| `hasUnloadedReplies` | Còn reply chưa được load | ☐ |

---

## Tính năng

- **Đa ngôn ngữ** — nhận diện comment tiếng Việt, Anh, Pháp, Tây Ban Nha, Đức, Hàn, Nhật
- **MutationObserver** — bắt mọi comment ngay khi xuất hiện trong DOM, kể cả khi Facebook xóa khỏi viewport (virtual scroll)
- **Emoji/Sticker** — cào được comment chỉ có emoji (`😂😂`) hoặc chỉ có sticker
- **Comment ngắn** — không bỏ sót comment 1-5 ký tự như "Ok", "Môm", "+"
- **Retry tự động** — nếu lần đầu extract thất bại, sẽ retry ở lượt scan tiếp theo
- **Progressive scraping** — cuộn từng bước và cào ngay tại chỗ, không đợi đến cuối
- **Xuất Excel** — có cột Thread hiển thị cây comment trực quan (`┌ Main`, `└─ Reply`)
- **Kéo panel** — có thể di chuyển UI đến bất kỳ vị trí nào trên màn hình
- **Bộ đếm thời gian** — hiển thị thời gian cào theo từng giây
- **Tô màu viền** — comment đã cào được tô màu theo cấp: đỏ=main, cam=reply 1, vàng=reply 2, xanh=reply 3+

---

## Debugging

Bật debug mode bằng cách chạy trong console (F12):

```javascript
window.DEPTH_DEBUG = true
```

Sẽ hiện log chi tiết về quá trình detect depth và extract author.

---

## Version History

### v1.3 (Hiện tại)
- ✅ Fix emoji-only comment (`😂😂😂`) — Facebook render emoji là `<img alt>`, textContent bị rỗng
- ✅ Fix comment thường bị miss lẻ — chỉ đánh dấu đã xử lý sau khi extract thành công, cho phép retry
- ✅ Thêm field `tagName` (người được @tag) và `stickerUrl`
- ✅ Dialog có thể kéo di chuyển (draggable)
- ✅ Bộ đếm thời gian cào (mm:ss)

### v1.2
- ✅ MutationObserver — bắt article ngay khi vào DOM, fix virtual scroll unload
- ✅ Tăng tốc 3-5x — giảm toàn bộ sleep(), dùng `instant` thay vì `smooth` scroll
- ✅ Xuất Excel (.xlsx) với SheetJS
- ✅ UI chọn fields và định dạng export ngay trên panel
- ✅ Deduplication bằng DOM element reference (thay vì outerHTML không ổn định)
- ✅ Fix comment ngắn (`Môm`, `Ok`) bị bỏ qua do threshold `> 5` ký tự
- ✅ Thêm keyword tiếng Việt: "Trả lời", "Bình luận", "Phản hồi"

### v1.1
- ✅ Tự động tải file JSON khi xong, không cần xác nhận
- ✅ Progressive scraping — cuộn + cào đồng thời

### v1.0
- 🎉 Phát hành đầu tiên
- ✅ Hỗ trợ reply lồng nhiều cấp
- ✅ Tô màu comment theo độ sâu
- ✅ 5 chiến lược detect depth

---

## License

MIT — mã nguồn mở, tự do sử dụng và đóng góp.
