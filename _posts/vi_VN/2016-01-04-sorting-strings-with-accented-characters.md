---
layout: post

title: Sắp xếp chuỗi với các ký tự có dấu
tip-number: 04
tip-username: loverajoel 
tip-username-profile: https://github.com/loverajoel
tip-tldr: Javascript has a native method **sort** that allows sorting arrays. Doing a simple `array.sort()` will treat each array entry as a string and sort it alphabetically. But when you try order an array of non ASCII characters you will obtain a strange result.

categories:
    - vi_VN
---

Javascript có 1 phương thức **[sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)** cho phép sắp xếp mảng. Cách sử dụng đơn giản `array.sort()` nếu một mảng là chuỗi thì sẽ sắp sếp theo abc. Ngoài ra bạn có thể cung cấp các  [tham số đầu vào](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort#Parameters) cho  function .

```javascript
['Shanghai', 'New York', 'Mumbai', 'Buenos Aires'].sort();
// ["Buenos Aires", "Mumbai", "New York", "Shanghai"]
```

Nhưng khi bạn cố gắng sắp xếp một mảng ký tự không phải là ASCII như: `['é', 'a', 'ú', 'c']`, bạn sẽ nhận được kết quả: `['c', 'e', 'á', 'ú']`. Điều này xảy ra vì nó đang được hoạt động với ngôn ngữ English.

Một ví dụ tiếp theo:

```javascript
// Vietnamese
['Ha', 'Hạnh', 'Huệ', 'Hường'].sort();
// ["Ha", "Huệ", "Hường", "Hạnh"] // 1 điểm về chỗ :((

// Spanish
['único','árbol', 'cosas', 'fútbol'].sort();
// ["cosas", "fútbol", "árbol", "único"] // bad order

// German
['Woche', 'wöchentlich', 'wäre', 'Wann'].sort();
// ["Wann", "Woche", "wäre", "wöchentlich"] // bad order
```

May mắn thay là ECMAScript Internationalization API đã cung cấp hai cách để khắc phục vấn đề này [localeCompare](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare) and [Intl.Collator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Collator).

> Mỗi phương thức đều có các tham số cấu hình tùy chỉnh riêng.

### Sử dụng `localeCompare()`

```javascript
['único','árbol', 'cosas', 'fútbol'].sort(function (a, b) {
  return a.localeCompare(b);
});
// ["árbol", "cosas", "fútbol", "único"]

['Woche', 'wöchentlich', 'wäre', 'Wann'].sort(function (a, b) {
  return a.localeCompare(b);
});
// ["Wann", "wäre", "Woche", "wöchentlich"]
```

### Sử dụng `Intl.Collator()`

```javascript
['único','árbol', 'cosas', 'fútbol'].sort(Intl.Collator().compare);
// ["árbol", "cosas", "fútbol", "único"]

['Woche', 'wöchentlich', 'wäre', 'Wann'].sort(Intl.Collator().compare);
// ["Wann", "wäre", "Woche", "wöchentlich"]
```

- Đối với mỗi phương pháp mà bạn có thể tùy chỉnh vị trí.
- Theo như [Firefox](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare#Performance) Intl.Collator sẽ nhanh hơn khi so sánh các chuỗi có số lượng lớn.

Do đó khi bạn làm việc với các mảng hoặc chuỗi trong một ngôn ngữ khác ngoài tiếng anh, nhớ sử dụng phương pháp này để tránh sắp xếp không chính xác.