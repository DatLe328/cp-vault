# Mảng cộng dồn

## Mảng cộng dồn (Prefix Sum Array)

Mảng cộng dồn là một kỹ thuật trong lập trình giúp tính **tổng các phần tử trong một đoạn con** của mảng một cách nhanh chóng. Mảng cộng dồn được xây dựng bằng cách lưu trữ tổng các phần tử từ đầu mảng đến mỗi vị trí.

### Độ phức tạp:

- **Thời gian**:
    - O(n) để tính toán mảng prefix sum.
    - O(1) để truy vấn tổng trong đoạn [l ;r]
- **Không gian**: O(n)

## Mảng hiệu (Difference Array)

Mảng hiệu là một kỹ thuật dùng để thực hiện các **cập nhật đoạn con** của mảng một cách hiệu quả. Mảng hiệu lưu trữ sự thay đổi tại mỗi vị trí, giúp cập nhật nhiều phần tử trong mảng gốc chỉ bằng hai thao tác.

### Độ phức tạp:

- **Thời gian**:
    - O(1) để cập nhật một đoạn con.
    - O(n) để truy vấn giá trị một số bất kỳ.
- **Không gian**: O(n)

## **Ứng dụng**

- Không chỉ ứng dụng trên mảng một chiều mà mảng tổng cộng dồn và mảng hiệu còn có thể cải tiến lên mảng 2 chiều, 3 chiều,... hay một số cấu trúc dữ liệu như đồ thị DAG hay trên cây chúng ta sẽ học sau này.

https://cses.fi/problemset/task/1661