
Câu 1 – Hai lỗi nghiệp vụ ở code trên:
1. Không kiểm tra quyền sở hữu đơn hàng chỉ lấy productId rồi trừ số lượng tồn kho, nhưng không xác thực đơn hàng đó có thực sự thuộc về userId gửi lên hay không. Hậu quả: User A có thể gửi   
productId của User B để đặt hàng trái phép, đặc biệt nguy hiểm với các sản phẩm hạn chế hoặc có giá trị cao.
   2. Không kiểm soát song song/điều kiện cạnh tranh (race condition)
   Nếu nhiều request đặt hàng cùng lúc, stock có thể bị trừ sai (ví dụ từ 1 xuống -1) vì không có cơ chế khóa hoặc transaction an toàn. Hậu qua: User có thể đặt hàng nhiều lần vượt quá số lượng tồn kho, dẫn đến lạm dụng sản phẩm và gây ra tình trạng hết hàng.
câu 2 : với mỗi lỗi chỉ ra ở câu 1, đề xuất giải pháp:
   3. Xác thực quyền sở hữu đơn hàng
Khi tìm product, cần kiểm tra product.getUserId() có khớp với request.getUserId() không. Nếu không khớp thì từ chối. Có thể bổ sung tầng bảo mật (JWT, OAuth) để đảm bảo user chỉ truy cập dữ liệu của mình.
      4. Đảm bảo tính toàn vẹn dữ liệu khi cập nhật
Sử dụng transaction hoặc optimistic locking (ví dụ @Version trong JPA) để tránh race condition. Hoặc dùng cơ chế synchronized/atomic update trong database để đảm bảo stock không bị trừ sai khi nhiều request đồng thời. Nếu không kiểm soát tốt, có thể dẫn đến việc sản phẩm bị bán quá số lượng tồn kho, gây ra tình trạng hết hàng và ảnh hưởng đến uy tín của cửa hàng.
   Câu 3 – vào các đợt flash sale, khuyến mãi, hoặc các sự kiện đặc biệt,nhiểu người mua cùng 1 sản phẩm giảm giá có số lượng giới hạn,trong các lỗi trên,lỗi sẽ bộ lộ rõ nhất là:
      lỗi 2: Không kiểm soát song song/điều kiện cạnh tranh (race condition) vì nếu không kiểm soát tốt, có thể dẫn đến việc sản phẩm bị bán quá số lượng tồn kho, gây ra tình trạng hết hàng và ảnh hưởng đến uy tín của cửa hàng.
      
      
   




















;

Câu 1 – Hai lỗi nghiệp vụ

Không kiểm tra quyền sở hữu đơn hàng

Code chỉ lấy productId rồi trừ số lần quanlity, nhưng không xác thực đơn hàng đó có thực sự thuộc về productId gửi lên hay không.
Hậu quả: người  A có thể gửi orderId của người B để lấy sản phẩm trái phép

Không kiểm soát song song/điều kiện cạnh tranh (race condition)

Nếu nhiều request stock cùng lúc, stocks có thể bị trừ sai (ví dụ từ 1 xuống -1) vì không có cơ chế khóa hoặc transaction an toàn.
Hậu quả: kho có thể nhận nhiều lần cấp sản phẩm vượt quá số lần cho phép, dẫn đến lạm dụng sản phẩm.
Câu 2 – Giải pháp khắc phục

Xác thực quyền sở hữu đơn hàng

Khi tìm đơn hàng, cần kiểm tra đơn hàng.getProductId() có khớp với request.getProductId() không. Nếu không khớp thì từ chối.
Có thể bổ sung tầng bảo mật (JWT, OAuth) để đảm bảo người dùng chỉ truy cập dữ liệu của mình.

Đảm bảo tính toàn vẹn dữ liệu khi cập nhật

Sử dụng transaction hoặc optimistic locking (ví dụ @Version trong JPA) để tránh race condition.
Hoặc dùng cơ chế synchronized/atomic update trong database để đảm bảo remainingRefills không bị trừ sai khi nhiều request đồng thời.
Câu 3 – vào các đợt flash sale, khuyến mãi, hoặc các sự kiện đặc biệt,nhiểu người mua cùng 1 sản phẩm giảm giá có số lượng giới hạn,lỗi sẽ bộc lộ rõ nhất:
là lỗi bán quá số lượng tồn kho, liên quan trực tiếp đến quyền lợi của khách hàng. Nếu cấp sai sản phẩm hoặc cấp quá số lượng, hậu quả có thể là mất uy tín, khách hàng phàn nàn, thậm chí kiện tụng.

