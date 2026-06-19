### Bài 3 — Đọc hiểu & Dò lỗi qua Prompt (Phát hiện lỗi logic lặp) (100 điểm)

Mã lỗi ban đầu:

```java
public class DuplicateFinder {

    // Lỗi logic: j bắt đầu từ i dẫn đến việc phần tử tự so sánh với chính nó (arr[i] == arr[i] luôn đúng)
    public static Integer findDuplicate(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            for (int j = i; j < arr.length; j++) {
                if (arr[i] == arr[j]) {
                    return arr[i];
                }
            }
        }
        return null;
    }

}
```

Phân tích vì sao prompt thô "Mã này bị lỗi gì?" dễ bỏ sót lỗi:

- Prompt quá chung chung không hướng dẫn AI thực hiện kiểm thử cụ thể.
- Code hợp lệ về cú pháp và không gây lỗi biên (compile), nên AI có thể bỏ qua lỗi logic biên (so sánh phần tử với chính nó).
- Cần cung cấp vai trò là Code Auditor và một case kiểm thử minh họa để AI bắt lỗi logic.

Prompt tối ưu (gửi kèm mã và test cụ thể):

"Bạn là một Code Auditor. Tôi có hàm `findDuplicate(int[] arr)` như trên. Hãy:
1) Chỉ ra lỗi logic hiện tại và vì sao hàm luôn trả `arr[0]` cho `int[] test = {1,2,3,4}`.
2) Thực hiện 'mental test' với `test` ở trên và mô tả kết quả hiện tại.
3) Viết lại hàm để tìm phần tử trùng lặp đầu tiên, sử dụng `HashSet` để đạt O(N) thời gian.
4) Trả về mã Java hoàn chỉnh và nêu độ phức tạp thời gian/không gian của giải pháp mới."

Mã Java sửa (sử dụng `HashSet`):

```java
import java.util.HashSet;
import java.util.Set;

public class DuplicateFinder {

    // Trả về phần tử trùng lặp đầu tiên (theo thứ tự xuất hiện đầu tiên)
    // Sử dụng HashSet để đạt O(N) thời gian trung bình
    public static Integer findDuplicate(int[] arr) {
        if (arr == null || arr.length == 0) return null;
        Set<Integer> seen = new HashSet<>();
        for (int value : arr) {
            if (seen.contains(value)) {
                return value;
            }
            seen.add(value);
        }
        return null;
    }

}
```

Độ phức tạp: Thời gian O(N) trung bình, không gian O(N).
