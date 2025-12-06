---
layout: default
title: Phụ lục B
parent: Bắt đầu
nav_order: 7
---

# You Don't Know JS Yet: Bắt đầu - Ấn bản thứ 2
# Phụ lục B: Thực hành, Thực hành, Thực hành!

Trong phụ lục này, chúng ta sẽ khám phá một số bài tập và giải pháp gợi ý của chúng. Những bài tập này chỉ để *giúp bạn bắt đầu* thực hành các khái niệm từ cuốn sách.

## Thực hành So sánh

Hãy thực hành làm việc với các loại giá trị và so sánh (Chương 4, Trụ cột 3) nơi ép kiểu sẽ cần phải tham gia.

`scheduleMeeting(..)` nên nhận thời gian bắt đầu (ở định dạng 24 giờ dưới dạng chuỗi "hh:mm") và thời lượng cuộc họp (số phút). Nó sẽ trả về `true` nếu cuộc họp nằm hoàn toàn trong ngày làm việc (theo thời gian được chỉ định trong `dayStart` và `dayEnd`); trả về `false` nếu cuộc họp vi phạm giới hạn ngày làm việc.

```js
const dayStart = "07:30";
const dayEnd = "17:45";

function scheduleMeeting(startTime,durationMinutes) {
    // ..TODO..
}

scheduleMeeting("7:00",15);     // false
scheduleMeeting("07:15",30);    // false
scheduleMeeting("7:30",30);     // true
scheduleMeeting("11:30",60);    // true
scheduleMeeting("17:00",45);    // true
scheduleMeeting("17:30",30);    // false
scheduleMeeting("18:00",15);    // false
```

Hãy cố gắng tự giải quyết vấn đề này trước. Xem xét việc sử dụng các toán tử so sánh bằng và quan hệ, và cách ép kiểu ảnh hưởng đến mã này. Khi bạn có mã hoạt động, hãy *so sánh* (các) giải pháp của bạn với mã trong "Giải pháp Gợi ý" ở cuối phụ lục này.

## Thực hành Closure

Bây giờ hãy thực hành với closure (Chương 4, Trụ cột 1).

Hàm `range(..)` nhận một số làm đối số đầu tiên của nó, đại diện cho số đầu tiên trong một phạm vi số mong muốn. Đối số thứ hai cũng là một số đại diện cho kết thúc của phạm vi mong muốn (bao gồm cả nó). Nếu đối số thứ hai bị bỏ qua, thì một hàm khác sẽ được trả về mong đợi đối số đó.

```js
function range(start,end) {
    // ..TODO..
}

range(3,3);    // [3]
range(3,8);    // [3,4,5,6,7,8]
range(3,0);    // []

var start3 = range(3);
var start4 = range(4);

start3(3);     // [3]
start3(8);     // [3,4,5,6,7,8]
start3(0);     // []

start4(6);     // [4,5,6]
```

Hãy cố gắng tự giải quyết vấn đề này trước.

Khi bạn có mã hoạt động, hãy *so sánh* (các) giải pháp của bạn với mã trong "Giải pháp Gợi ý" ở cuối phụ lục này.

## Thực hành Nguyên mẫu

Cuối cùng, hãy làm việc trên `this` và các đối tượng được liên kết qua nguyên mẫu (Chương 4, Trụ cột 2).

Định nghĩa một máy đánh bạc (slot machine) với ba cuộn quay có thể `spin()` (quay) riêng lẻ, và sau đó `display()` (hiển thị) nội dung hiện tại của tất cả các cuộn quay.

Hành vi cơ bản của một cuộn quay đơn lẻ được định nghĩa trong đối tượng `reel` bên dưới. Nhưng máy đánh bạc cần các cuộn quay riêng lẻ—các đối tượng ủy quyền cho `reel`, và mỗi đối tượng có một thuộc tính `position` (vị trí).

Một cuộn quay chỉ *biết cách* `display()` biểu tượng khe hiện tại của nó, nhưng một máy đánh bạc thường hiển thị ba biểu tượng trên mỗi cuộn quay: khe hiện tại (`position`), một khe ở trên (`position - 1`), và một khe ở dưới (`position + 1`). Vì vậy, việc hiển thị máy đánh bạc sẽ kết thúc bằng việc hiển thị một lưới 3 x 3 các biểu tượng khe.

```js
function randMax(max) {
    return Math.trunc(1E9 * Math.random()) % max;
}

var reel = {
    symbols: [
        "♠", "♥", "♦", "♣", "☺", "★", "☾", "☀"
    ],
    spin() {
        if (this.position == null) {
            this.position = randMax(
                this.symbols.length - 1
            );
        }
        this.position = (
            this.position + 100 + randMax(100)
        ) % this.symbols.length;
    },
    display() {
        if (this.position == null) {
            this.position = randMax(
                this.symbols.length - 1
            );
        }
        return this.symbols[this.position];
    }
};

var slotMachine = {
    reels: [
        // máy đánh bạc này cần 3 cuộn quay riêng biệt
        // gợi ý: Object.create(..)
    ],
    spin() {
        this.reels.forEach(function spinReel(reel){
            reel.spin();
        });
    },
    display() {
        // TODO
    }
};

slotMachine.spin();
slotMachine.display();
// ☾ | ☀ | ★
// ☀ | ♠ | ☾
// ♠ | ♥ | ☀

slotMachine.spin();
slotMachine.display();
// ♦ | ♠ | ♣
// ♣ | ♥ | ☺
// ☺ | ♦ | ★
```

Hãy cố gắng tự giải quyết vấn đề này trước.

Gợi ý:

* Sử dụng toán tử modulo `%` để bao bọc `position` khi bạn truy cập các biểu tượng theo vòng tròn quanh một cuộn quay.

* Sử dụng `Object.create(..)` để tạo một đối tượng và liên kết nguyên mẫu nó với một đối tượng khác. Sau khi được liên kết, ủy quyền cho phép các đối tượng chia sẻ ngữ cảnh `this` trong quá trình gọi phương thức.

* Thay vì sửa đổi đối tượng cuộn quay trực tiếp để hiển thị từng vị trí trong ba vị trí, bạn có thể sử dụng một đối tượng tạm thời khác (`Object.create(..)` một lần nữa) với `position` riêng của nó, để ủy quyền từ đó.

Khi bạn có mã hoạt động, hãy *so sánh* (các) giải pháp của bạn với mã trong "Giải pháp Gợi ý" ở cuối phụ lục này.

## Giải pháp Gợi ý

Hãy nhớ rằng những giải pháp gợi ý này chỉ là: gợi ý. Có nhiều cách khác nhau để giải quyết các bài tập thực hành này. So sánh cách tiếp cận của bạn với những gì bạn thấy ở đây, và xem xét ưu và nhược điểm của mỗi cách.

Giải pháp gợi ý cho thực hành "So sánh" (Trụ cột 3):

```js
const dayStart = "07:30";
const dayEnd = "17:45";

function scheduleMeeting(startTime,durationMinutes) {
    var [ , meetingStartHour, meetingStartMinutes ] =
        startTime.match(/^(\d{1,2}):(\d{2})$/) || [];

    durationMinutes = Number(durationMinutes);

    if (
        typeof meetingStartHour == "string" &&
        typeof meetingStartMinutes == "string"
    ) {
        let durationHours =
            Math.floor(durationMinutes / 60);
        durationMinutes =
            durationMinutes - (durationHours * 60);
        let meetingEndHour =
            Number(meetingStartHour) + durationHours;
        let meetingEndMinutes =
            Number(meetingStartMinutes) +
            durationMinutes;

        if (meetingEndMinutes >= 60) {
            meetingEndHour = meetingEndHour + 1;
            meetingEndMinutes =
                meetingEndMinutes - 60;
        }

        // biên soạn lại các chuỗi thời gian đủ điều kiện
        // (để làm cho việc so sánh dễ dàng hơn)
        let meetingStart = `${
            meetingStartHour.padStart(2,"0")
        }:${
            meetingStartMinutes.padStart(2,"0")
        }`;
        let meetingEnd = `${
            String(meetingEndHour).padStart(2,"0")
        }:${
            String(meetingEndMinutes).padStart(2,"0")
        }`;

        // LƯU Ý: vì các biểu thức đều là chuỗi,
        // so sánh ở đây là theo thứ tự bảng chữ cái, nhưng nó
        // an toàn ở đây vì chúng là các chuỗi thời gian
        // đủ điều kiện (tức là, "07:15" < "07:30")
        return (
            meetingStart >= dayStart &&
            meetingEnd <= dayEnd
        );
    }

    return false;
}

scheduleMeeting("7:00",15);     // false
scheduleMeeting("07:15",30);    // false
scheduleMeeting("7:30",30);     // true
scheduleMeeting("11:30",60);    // true
scheduleMeeting("17:00",45);    // true
scheduleMeeting("17:30",30);    // false
scheduleMeeting("18:00",15);    // false
```

----

Giải pháp gợi ý cho thực hành "Closure" (Trụ cột 1):

```js
function range(start,end) {
    start = Number(start) || 0;

    if (end === undefined) {
        return function getEnd(end) {
            return getRange(start,end);
        };
    }
    else {
        end = Number(end) || 0;
        return getRange(start,end);
    }


    // **********************

    function getRange(start,end) {
        var ret = [];
        for (let i = start; i <= end; i++) {
            ret.push(i);
        }
        return ret;
    }
}

range(3,3);    // [3]
range(3,8);    // [3,4,5,6,7,8]
range(3,0);    // []

var start3 = range(3);
var start4 = range(4);

start3(3);     // [3]
start3(8);     // [3,4,5,6,7,8]
start3(0);     // []

start4(6);     // [4,5,6]
```

----

Giải pháp gợi ý cho thực hành "Nguyên mẫu" (Trụ cột 2):

```js
function randMax(max) {
    return Math.trunc(1E9 * Math.random()) % max;
}

var reel = {
    symbols: [
        "♠", "♥", "♦", "♣", "☺", "★", "☾", "☀"
    ],
    spin() {
        if (this.position == null) {
            this.position = randMax(
                this.symbols.length - 1
            );
        }
        this.position = (
            this.position + 100 + randMax(100)
        ) % this.symbols.length;
    },
    display() {
        if (this.position == null) {
            this.position = randMax(
                this.symbols.length - 1
            );
        }
        return this.symbols[this.position];
    }
};

var slotMachine = {
    reels: [
        Object.create(reel),
        Object.create(reel),
        Object.create(reel)
    ],
    spin() {
        this.reels.forEach(function spinReel(reel){
            reel.spin();
        });
    },
    display() {
        var lines = [];

        // hiển thị tất cả 3 dòng trên máy đánh bạc
        for (
            let linePos = -1; linePos <= 1; linePos++
        ) {
            let line = this.reels.map(
                function getSlot(reel){
                    var slot = Object.create(reel);
                    slot.position = (
                        reel.symbols.length +
                        reel.position +
                        linePos
                    ) % reel.symbols.length;
                    return slot.display();
                }
            );
            lines.push(line.join(" | "));
        }

        return lines.join("\n");
    }
};

slotMachine.spin();
slotMachine.display();
// ☾ | ☀ | ★
// ☀ | ♠ | ☾
// ♠ | ♥ | ☀

slotMachine.spin();
slotMachine.display();
// ♦ | ♠ | ♣
// ♣ | ♥ | ☺
// ☺ | ♦ | ★
```

Đó là tất cả cho cuốn sách này. Nhưng bây giờ đã đến lúc tìm kiếm các dự án thực tế để thực hành những ý tưởng này. Chỉ cần tiếp tục viết mã, vì đó là cách tốt nhất để học!
