---
layout: post 
title: Giải thuật cơ bản về rust trên LeetCode
group: blockchain
state: publish

---

Đây là chuyên mục cùng học Rust với mình, mình biết Rust cũng được một thời gian nhưng bây giờ mới thực sự học về nó. 
Để trau dồi kiến thức mình sẽ luyện tập với Rust, các đáp án dưới đây chỉ mang tính tham khảo, có thể do mình làm hoặc 
mình sưu tầm được, nếu có ý kiến đóng góp hãy để lại bình luận bên dưới.

# Danh sách các bài tập đã làm: 

1- [Two Sum](#twosum)

9- [Palindrome Number](palindromenumber)

13- [Roman to Integer](romantointeger)

14- [Longest Common Prefix](longestcommonprefix)

# 1. Two Sum <a name="twosum"></a>

***Cho một mảng interges `nums` and 1 số nguyên `target`, trả về vị trí của 2 số trong mảng có tổng bằng 6***

**Example:**{: style="color: orange; opacity: 0.80;" }

```
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1] // Because nums[0] + nums[1] == 9, => Return [0, 1]
```

**Answer**{: style="color: red; opacity: 0.50;" }

```rust
use std::collections::HashMap;

impl Solution {
    pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
        
        let mut data: HashMap<i32, i32> = HashMap::new();
        
        for i in 0..nums.len(){
            match data.get(&nums[i]){
                Some(&x) => return vec![x, i as i32],
                None => data.insert(target-nums[i], i as i32),
            };
    }
    
    return vec![-1, -1]
    }
}
```

Note: Khởi tạo 1 dictionary, chạy vòng lặp qua tất cả giá trị của Vec `nums`, nếu giá trị `nums[i]`
có trong dictionary thì trả về kết quả, còn không thì insert giá trị `target - nums[i]` vào dictionary


# 9. Palindrome Number <a name="palindromenumber"></a>

***Cho 1 số integer x, trả về true nếu là số palindrome***

**Example**{: style="color: orange; opacity: 0.80;" }

```markdown
Input: x = 121, x = 12321, x = 11
Ouput: true 
Input: x = 123, 2342
Ouput: false
```

**Answer**{: style="color: red; opacity: 0.50;" }

```rust
impl Solution {
    pub fn is_palindrome(x: i32) -> bool {
        let x_str: String = x.to_string();
        
        let revert: String = x_str.chars().rev().collect::<String>();
        
        return x_str == revert;
    }
}

```

# 13. Roman to Integer <a name="romantointeger"></a>

***Chữ số Roman được đại diện bằng 7 ký tự khác nhau: I, V, X, L, C, D và M.***

```markdown
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

Cho 1 chữ số Roman, chuyển đổi sang integer 

**Example**{: style="color: orange; opacity: 0.80;" }

```markdown
Input: s = "XIX"
Ouput: 19
```

**Answer**{: style="color: red; opacity: 0.50;" }

```rust
use std::collections::HashMap;

impl Solution {
    pub fn roman_to_int(s: String) -> i32 {
        
        let values = s.chars().map(|x| match x {
            'I' => 1,
            'V' => 5,
            'X' => 10,
            'L' => 50,
            'C' => 100,
            'D' => 500,
            'M' => 1000,
            _ => 0,
        
        }).collect::<Vec<i32>>();
        
        let mut sum = values[0];
        
        for i in 0..values.len() -1 {
            if values[i] >= values[i+1]{
                sum += values[i+1]; // XI = X + I
            }else {
                sum += values[i+1] - 2*values[i]; //XIX = (X+I) - I +X - I 
            }
        }
        
        return sum
            
}   

}
```

# 14. Longest Common Prefix <a name="longestcommonprefix"></a>

***Viết 1 function trả về chuỗi prefix chung lớn nhất của 1 mảng string.
Nếu không có prefix chung, trả về "".***

**Example**{: style="color: orange; opacity: 0.80;" }
```markdown
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

**Answer**{: style="color: red; opacity: 0.50;" }

```rust
impl Solution {
    pub fn longest_common_prefix(strs: Vec<String>) -> String {
        
        let min_length = strs.iter().map(|x| x.len()).min().unwrap();

        for i in (1..min_length+1).rev() {
            let prefix = &strs[0][0..i];
            if strs.iter().all(|x| x.find(prefix) == Some(0)){
                return prefix.to_string();
            }

            }

        String::new()
        }
}

```


# Tài liệu 

- https://leetcode.com/