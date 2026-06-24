# BÀI 2: Thực hành Xây dựng Thuật toán Khuyến mãi Phức tạp (Chain-of-thought - CoT) 
## Phần giải thích của bạn về tầm quan trọng của thứ tự ưu tiên tính toán trong các nghiệp vụ kế toán/tài chính
- Đảm bảo tuân thủ quy định thuế: VAT phải được tính trên giá trị hàng hóa sau khi áp dụng các khoản giảm giá hợp lệ.
- Tránh thất thoát doanh thu do áp dụng sai thứ tự chiết khấu.
- Đảm bảo công bằng giữa khách hàng và doanh nghiệp.
- Giúp kết quả tính toán nhất quán trên mọi nền tảng (Web, Mobile, POS).
- Tránh sai lệch số liệu kế toán và báo cáo tài chính.
## Nội dung Prompt CoT do bạn thiết kế
- Vai trò: Bạn là Senior Solution Architect chuyên về hệ thống thương mại điện tử và kế toán tài chính.

      BƯỚC 1 - Phân tích nghiệp vụ
      - Xác định đúng thứ tự thực hiện các quy tắc tính tiền.
      - Giải thích vì sao từng bước phải đứng trước hoặc sau bước khác.
      - Trình bày dưới dạng danh sách đánh số.
      
      BƯỚC 2 - Xây dựng công thức
      Cho mỗi bước:
      - Nêu công thức tính.
      - Nêu đầu vào.
      - Nêu đầu ra.
      
      Các quy tắc:
      
      1. Tính tổng tiền gốc giỏ hàng.
      2. Áp dụng Product Discount.
      3. Áp dụng Coupon:
         - Giảm 10% tổng đơn.
         - Tối đa 100,000 VND.
         - Chỉ áp dụng khi giá trị đơn hàng sau Product Discount >= 500,000 VND.
      4. Áp dụng Loyalty Discount:
         - Gold: 5%
         - Platinum: 10%
      5. Tính VAT 10%.
      6. Tính Shipping:
         - Dưới hoặc bằng 10km: 30,000 VND.
         - Mỗi km vượt quá 10km cộng thêm 5,000 VND.
         - Miễn phí vận chuyển nếu tổng tiền sau tất cả chiết khấu và trước VAT > 1,000,000 VND.
      
      BƯỚC 3 - Dry Run
      Thực hiện tính toán từng bước với dữ liệu:
      
      - Product A:
        + Số lượng: 2
        + Giá gốc: 400,000
        + Giảm trực tiếp: 10%
      
      - Product B:
        + Số lượng: 1
        + Giá gốc: 300,000
        + Không giảm giá
      
      - Coupon:
        + Giảm 50,000 VND
        + Điều kiện đơn tối thiểu 500,000 VND
      
      - Khách hàng:
        + Hạng Gold
      
      - Khoảng cách giao hàng:
        + 12 km
      
      Yêu cầu:
      - Hiển thị từng phép tính.
      - Hiển thị kết quả sau mỗi bước.
      - Kết luận số tiền cuối cùng.
      
      BƯỚC 4 - Sinh mã nguồn Java
      
      Sinh class OrderCalculator với:
      
      public BigDecimal calculateTotal(Order order)
      
      Yêu cầu:
      - Dùng BigDecimal.
      - Không dùng double.
      - Tách nhỏ các hàm:
        + calculateSubtotal()
        + applyProductDiscount()
        + applyCoupon()
        + applyLoyaltyDiscount()
        + calculateVAT()
        + calculateShipping()
      - Có comment giải thích.
      - Tuân thủ thứ tự nghiệp vụ đã phân tích.
## Minh chứng chạy thực tế

    BƯỚC 1 - Phân tích thứ tự xử lý
    Thứ tự đúng
    Tính tổng tiền gốc của giỏ hàng.
    Áp dụng Product Discount.
    Áp dụng Coupon.
    Áp dụng Loyalty Discount.
    Tính VAT.
    Tính Shipping Fee.
    Tính tổng thanh toán cuối cùng.
    Giải thích
    
    Product Discount trước Coupon
    
    Coupon phải áp dụng trên giá trị thực tế của sản phẩm sau khi đã giảm giá.
    
    Coupon trước Loyalty
    
    Loyalty Discount được tính trên số tiền khách hàng thực sự phải trả sau khi sử dụng coupon.
    
    Loyalty trước VAT
    
    Thuế phải tính trên giá trị hàng hóa cuối cùng sau các khoản giảm giá hợp lệ.
    
    VAT trước Total
    
    VAT là nghĩa vụ thuế phát sinh từ giá trị hàng hóa sau giảm giá.
    
    Shipping cuối cùng
    
    Phí vận chuyển không phải là hàng hóa chịu ảnh hưởng của các chương trình giảm giá trong bài toán này.
    BƯỚC 2 - Công thức
    Tổng tiền gốc
    Subtotal = Σ(UnitPrice × Quantity)
    Product Discount
    AfterProductDiscount
    = Subtotal - ProductDiscountAmount
    Coupon
    CouponAmount
    = min(10% × AfterProductDiscount, 100,000)
    
    AfterCoupon
    = AfterProductDiscount - CouponAmount
    Loyalty
    Gold = 5%
    
    AfterLoyalty
    = AfterCoupon - (AfterCoupon × 5%)
    VAT
    VAT
    = AfterLoyalty × 10%
    Shipping
    Nếu AfterLoyalty > 1,000,000
    
    Shipping = 0
    
    Ngược lại
    
    Shipping = 30,000 + (Distance - 10) × 5,000
    Tổng cuối
    FinalTotal
    = AfterLoyalty + VAT + Shipping
    BƯỚC 3 - Dry Run
    Tính tổng tiền gốc
    
    Sản phẩm A
    
    400,000 × 2
    = 800,000
    
    Sản phẩm B
    
    300,000 × 1
    = 300,000
    
    Tổng tiền gốc
    
    800,000 + 300,000
    = 1,100,000
    Product Discount
    
    Sản phẩm A giảm 10%
    
    800,000 × 10%
    = 80,000
    
    Giá trị sau giảm sản phẩm
    
    1,100,000 - 80,000
    = 1,020,000
    Coupon
    
    Điều kiện:
    
    1,020,000 >= 500,000
    
    => Đủ điều kiện.
    
    Coupon khách hàng nhập:
    
    50,000
    
    Sau Coupon
    
    1,020,000 - 50,000
    = 970,000
    Loyalty Discount (Gold 5%)
    970,000 × 5%
    = 48,500
    
    Sau Loyalty
    
    970,000 - 48,500
    = 921,500
    VAT 10%
    921,500 × 10%
    = 92,150
    Shipping
    
    Khoảng cách:
    
    12 km
    
    Phí cơ bản:
    
    30,000
    
    Phần vượt:
    
    12 - 10 = 2 km
    
    Phụ phí:
    
    2 × 5,000 = 10,000
    
    Shipping:
    
    30,000 + 10,000
    = 40,000
    Tổng cuối cùng
    921,500
    + 92,150
    + 40,000
    = 1,053,650
    KẾT QUẢ
    Tổng thanh toán cuối cùng = 1,053,650 VND
    BƯỚC 4 - Class Java OrderCalculator
    import java.math.BigDecimal;
    import java.math.RoundingMode;
    
    public class OrderCalculator {

    private static final BigDecimal VAT_RATE =
            new BigDecimal("0.10");

    private static final BigDecimal GOLD_RATE =
            new BigDecimal("0.05");

    private static final BigDecimal PLATINUM_RATE =
            new BigDecimal("0.10");

    private static final BigDecimal FREE_SHIPPING_THRESHOLD =
            new BigDecimal("1000000");

    private static final BigDecimal BASE_SHIPPING =
            new BigDecimal("30000");

    private static final BigDecimal EXTRA_KM_FEE =
            new BigDecimal("5000");

    public BigDecimal calculateTotal(Order order) {

        BigDecimal subtotal =
                calculateSubtotal(order);

        BigDecimal afterProductDiscount =
                applyProductDiscount(order, subtotal);

        BigDecimal afterCoupon =
                applyCoupon(order, afterProductDiscount);

        BigDecimal afterLoyalty =
                applyLoyaltyDiscount(order, afterCoupon);

        BigDecimal vat =
                calculateVAT(afterLoyalty);

        BigDecimal shipping =
                calculateShipping(
                        order,
                        afterLoyalty
                );

        return afterLoyalty
                .add(vat)
                .add(shipping);
    }

    public BigDecimal calculateSubtotal(Order order) {
        BigDecimal total = BigDecimal.ZERO;

        for (OrderItem item : order.getItems()) {
            total = total.add(
                    item.getPrice()
                        .multiply(
                                BigDecimal.valueOf(
                                        item.getQuantity()
                                )
                        )
            );
        }

        return total;
    }

    public BigDecimal applyProductDiscount(
            Order order,
            BigDecimal subtotal) {

        BigDecimal discount =
                BigDecimal.ZERO;

        for (OrderItem item : order.getItems()) {

            BigDecimal itemTotal =
                    item.getPrice()
                        .multiply(
                                BigDecimal.valueOf(
                                        item.getQuantity()
                                )
                        );

            BigDecimal itemDiscount =
                    itemTotal.multiply(
                            item.getProductDiscountRate()
                    );

            discount = discount.add(itemDiscount);
        }

        return subtotal.subtract(discount);
    }

    public BigDecimal applyCoupon(
            Order order,
            BigDecimal amount) {

        if (order.getCoupon() == null) {
            return amount;
        }

        if (amount.compareTo(
                new BigDecimal("500000")) < 0) {
            return amount;
        }

        return amount.subtract(
                order.getCoupon().getDiscountAmount()
        );
    }

    public BigDecimal applyLoyaltyDiscount(
            Order order,
            BigDecimal amount) {

        BigDecimal rate = BigDecimal.ZERO;

        switch (order.getCustomerLevel()) {

            case GOLD:
                rate = GOLD_RATE;
                break;

            case PLATINUM:
                rate = PLATINUM_RATE;
                break;

            default:
                return amount;
        }

        BigDecimal discount =
                amount.multiply(rate);

        return amount.subtract(discount);
    }

    public BigDecimal calculateVAT(
            BigDecimal amount) {

        return amount.multiply(VAT_RATE)
                .setScale(
                        0,
                        RoundingMode.HALF_UP
                );
    }

    public BigDecimal calculateShipping(
            Order order,
            BigDecimal afterDiscount) {

        if (afterDiscount.compareTo(
                FREE_SHIPPING_THRESHOLD) > 0) {
            return BigDecimal.ZERO;
        }

        int distance = order.getDistanceKm();

        if (distance <= 10) {
            return BASE_SHIPPING;
        }

        int extraKm = distance - 10;

        return BASE_SHIPPING.add(
                EXTRA_KM_FEE.multiply(
                        BigDecimal.valueOf(extraKm)
                )
        );
    }
    }
