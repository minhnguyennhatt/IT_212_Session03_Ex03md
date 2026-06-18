# Prompt đưa vào Antigravity

Bạn hãy đóng vai trò là **System Analyst** trong dự án **Guai-api**. Hệ thống đã index mã nguồn hiện tại, bao gồm đoạn code trong `CartService.java` xử lý chức năng thêm sản phẩm vào giỏ hàng.

## Mục tiêu

Hãy phân tích logic hiện tại của hàm `addToCart(Long userId, Long productId, int quantity)` trong `CartService.java`, sau đó thực hiện đồng thời 2 nhiệm vụ sau:

1. **Chỉ ra các lỗ hổng logic trong code hiện tại**.
2. **Xuất ra danh sách Quy tắc nghiệp vụ (Business Rules) theo chuẩn SRS** để bộ phận Dev có căn cứ thực hiện bản vá.

## Ngữ cảnh nghiệp vụ

Module Giỏ hàng cho phép người dùng thêm sản phẩm vào giỏ hàng. Khi người dùng thêm sản phẩm, hệ thống cần kiểm tra sản phẩm có tồn tại, số lượng nhập vào có hợp lệ và số lượng sau khi thêm không được vượt quá tồn kho hiện có của sản phẩm.

Đoạn code hiện tại đã kiểm tra sản phẩm có tồn tại thông qua `productRepository.findById(productId)`, nhưng có nguy cơ thiếu các kiểm tra nghiệp vụ quan trọng liên quan đến biến `quantity` và tồn kho sản phẩm.

## Yêu cầu phân tích lỗ hổng logic

Hãy phân tích và chỉ rõ các vấn đề sau trong code hiện tại:

- Code chưa kiểm tra `quantity` có nhỏ hơn hoặc bằng 0 hay không.
- Nếu `quantity` là số âm, hệ thống có thể làm giảm số lượng sản phẩm trong giỏ hàng một cách bất hợp lý.
- Code chưa kiểm tra `quantity` có vượt quá số lượng tồn kho của sản phẩm hay không.
- Nếu sản phẩm đã tồn tại trong giỏ hàng, code chỉ cộng thêm số lượng mới mà chưa kiểm tra tổng số lượng sau khi cộng có vượt quá tồn kho hay không.
- Code chưa có exception rõ ràng cho các trường hợp số lượng không hợp lệ hoặc vượt tồn kho.
- Code có thể cho phép lưu dữ liệu giỏ hàng sai nghiệp vụ vào database.

## Yêu cầu xuất Business Rules theo chuẩn SRS

Sau khi phân tích lỗi, hãy xuất ra danh sách **Business Rules** theo định dạng bảng gồm các cột:

- Mã quy tắc
- Tên quy tắc
- Mô tả quy tắc nghiệp vụ
- Điều kiện áp dụng
- Hành vi hệ thống mong muốn
- Mức độ ưu tiên

## Ràng buộc bắt buộc

Danh sách Business Rules bắt buộc phải bao gồm tối thiểu các quy tắc sau:

1. `quantity` khi thêm vào giỏ hàng phải là số nguyên dương và lớn hơn 0.
2. Sản phẩm phải tồn tại trong hệ thống trước khi được thêm vào giỏ hàng.
3. Số lượng sản phẩm thêm mới không được vượt quá số lượng tồn kho hiện có.
4. Nếu sản phẩm đã có trong giỏ hàng, tổng số lượng sau khi cộng thêm không được vượt quá tồn kho.
5. Nếu số lượng không hợp lệ, hệ thống phải từ chối thao tác và trả về lỗi rõ ràng.
6. Nếu số lượng vượt tồn kho, hệ thống phải từ chối thao tác và thông báo số lượng không đủ.
7. Hệ thống chỉ được lưu `CartItem` khi tất cả điều kiện nghiệp vụ đều hợp lệ.

## Định dạng đầu ra mong muốn

Hãy trình bày kết quả theo cấu trúc sau:

## 1. Phân tích lỗ hổng logic trong code hiện tại

Liệt kê từng lỗ hổng, giải thích nguyên nhân và hậu quả có thể xảy ra.

## 2. Business Rules - Module Cart / Add To Cart

Trình bày bảng Business Rules theo format SRS.

## 3. Gợi ý hướng xử lý cho Dev

Đề xuất ngắn gọn các bước Dev cần bổ sung trong code, ví dụ:

- Kiểm tra `quantity <= 0`.
- Kiểm tra tồn kho của sản phẩm.
- Kiểm tra tổng số lượng trong giỏ hàng sau khi cộng thêm.
- Throw exception phù hợp như `InvalidQuantityException` hoặc `InsufficientInventoryException`.
- Chỉ gọi `cartRepository.save(item)` sau khi toàn bộ điều kiện hợp lệ.
