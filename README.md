# Memorizing Game

A simple converter for RGB code transfer to Hex code

## Features

遊戲規則說明如下：

一副撲克牌共有 52 張牌，分別為黑桃、紅心、方塊、梅花 4 種花色，每種花色有 13 張牌，分別為 1～13 點。

遊戲開始時，牌桌上覆蓋 52 張牌，玩家需要翻開牌面才能得知點數。

玩家一次只能翻開兩張牌，若兩張牌點數一樣，則為配對成功，配對成功的牌可以保留翻開的狀態；若配對失敗，則卡片會重新覆蓋，玩家要趁機記住卡片的號碼，尋求下一次配對成功的機會。

玩家的任務是將 52 張牌／26 組對子全部翻開。每成功配對一次可以得到 10 分，累積 260 分即達成任務。



## Function used

- MVC structure 
- Fisher-Yates Shuffle 洗牌演算法
``` js
getRandomNumberArray (count) {
    const number = Array.from(Array(count).keys())
    for (let index = number.length - 1; index > 0; index--) {
      let randomIndex = Math.floor(Math.random() * (index + 1))
        ;[number[index], number[randomIndex]] = [number[randomIndex], number[index]]
    }
    return number
  }
```
- 迭代器（Iterator）搭配 map 用法
``` js
Array.from(Array(52).keys()).map(index => this.getCardElement(index)).join("");
```
用 map 迭代陣列，並依序將數字丟進 view.getCardElement()，會變成有 52 張卡片的陣列；
接著要用 join("") 把陣列合併成一個大字串，才能當成 HTML template 來使用；

- setTimeour
``` js
setTimeout(() => {
            view.flipCard(model.revealedCards[0])
            view.flipCard(model.revealedCards[1])
            model.revealedCards = []
            this.currentState = GAME_STATE.FirstCardAwaits
          }, 1000)
```
- [...]
把參數轉變成一個陣列來迭代
``` js
flipCards (...cards) {
    cards.map(card => {
      if (card.classList.contains('back')) {
        card.classList.remove('back')
        card.innerHTML = this.getCardContent(Number(card.dataset.index))
        return
      }
      card.classList.add('back')
      card.innerHTML = null
    })
  },
```
- CSS animations : @keyframes
```
.wrong {
  animation-name: wrongAnimation;
  animation-duration: 0.2s;
  animation-iteration-count: 5;
}
@keyframes wrongAnimation {
  to {
    border: 2px solid #ffd54f;
  }
}
```

- {once: true} 
```
card.addEventListener('animationend', event =>   event.target.classList.remove('wrong'), { once: true })
    })
```
要求在事件執行一次之後，就要卸載這個監聽器。