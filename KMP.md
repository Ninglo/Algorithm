# KMP 算法

## 题目地址

[实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

## 前言

KMP 算法是一种使用 DFA 实现的经典代码。 在理解状态机的前提下其优雅易懂，但是单纯为学习此算法学习 DFA 似乎亦不太值当。 因而给出一种不需理解 DFA 概念的解释方式。

首先需要明确的是，KMP 算法本身思路清奇，一般人很难想象的到这种算法。我们做的应该是理解其思路， 争取在面试时将其快速清晰的编写出来即可。（源代码在最底部）

## 引入

首先我们考虑此问题的暴力解法：两层 for 循环，对待检测字符串切片，取得所有等于匹配字符串长度的字符串， 与匹配字符串进行比较。 由于切片会取得 `O(n)` 数量的字符串， 每个字符串与匹配字符串比较会耗费 `O(m)` 的时间， 所以总的用时为 `O(n*m)`。

## 改善思考

显然暴力解法存在改善空间：匹配字符串之间存在重复的部分（如 'aab' 匹配 'aaa' 后还可保留 'aa' 继续匹配，但暴力法会将这部分字符全部丢弃），但是检测时并没有利用此信息。如果使用某种数据结构存储这部分信息，就可以让每个字符的对比时间降为 `O(1)`，从而将对比的复杂度降到 `O(n)`。 此时只要保证构造数据结构的时间为 `O(m)` 级别，那么总的复杂度就是 `O(m+n)`。

## DFA 使用方法介绍

首先， 我们把 `KMP()` 返回的 DFA 当成一个黑盒， 观察其类型定义 `type DFA = Array<Map<string, number>>`  可知， DFA 是一个包含多个 Map 的数组。 在此首先解释本算法中 DFA 的使用方法， 代码片段如下：

```JavaScript
1 for (let matchLength = 0， i=0; i < txt.length; i++) {
2     const chr = txt[i];
3     matchLength = DFA[matchLength].get(chr) || 0;
4     if (matchLength === pat.length) return i - pat.length + 1;
5 }
```

现在用通俗的语言将第三行代码进行翻译：在匹配字符串与某个待检测字符串已经匹配了前 `matchLength` 个字符的前提下，输入新的一个 `chr` 字符， DFA 返回目前匹配的字符数量。

显然，如果成功构造了 DFA， 那么只需遍历一遍匹配字符串，就可以找到第一次匹配的位点。 那么下一步就是思考， 如何构造出一个符合我们定义的 DFA 结构。

## DFA 的构造

分析上文给出的通俗定义，“在匹配字符串与某个待检测字符串已经匹配了前 `matchLength` 个字符的前提下，输入新的一个 `chr` 字符， DFA 返回目前匹配的字符数量。”。从最基础的 `matchLength = 0` 开始，如果已经匹配了 0 个字符，那么 `DFA[0].get(chr)` 的返回值必然有 `chr === pat[0] ? 1 : 0` 的关系存在。 因而很容易得知， DFA 的第一个 Map 为 `{ a: 1 }` （其中 a 即 `pat[0]` 的值）。 推广可知，第i个 Map 中必存在 `{ a: i+1 }` （其中 a 即 `pat[i]` 的值）。 此部分构造 DFA 的代码如下：

```TypeScript
const M = pat.length;
const DFA: DFA = [];
for (let i=0; i<M; i++) {
    const chr = pat[i];
    const map: Map<string, number> = new Map();
    map.set(chr, i+1);
    DFA.push(map);
}
```

上述推理得出的 DFA 在字符永远匹配成功时适用。但考虑一特殊情况， `pat = 'aabaab'`, 当 `matchLength = 2， chr='a'` （即此部分切片为 'aaa'） 时，按照定义应返回 `matchLength = 2`(匹配 'aa')。但是我们构造的 DFA 由于没有对此情况的处理， 所以只会返回 0。分析原因, 是由于我们目前的 DFA 还没有实现 [改善思考](#改善思考) 中提到的 `利用匹配字符串之间存在重复的部分`。

这部分的处理方法， 也是 KMP 算法最精彩的一部分。首先，我们知道 DFA 中的每个 Map 中的 keys 实际上就是匹配字符串中字符的集合（默认返回值为0）。而在上一段中， 我们已经为 DFA 构造好了匹配成功的部分。现在，我们只需要考虑当某个字符匹配失败后，目前是仍有部分字符可以匹配（如 'aaa' 匹配 'aab' 失败， 但是还可保留 'aa' 部分），还是彻底重新开始匹配（如 'ab' 匹配 'aa')。

如以下代码所示，一种及其精彩的处理办法就是维护一个新的字符串string，其为`pat[i-matchLength:i]` 的最后 `matchLength` 项，并有`string = pat[0: matchLength]`。

```TypeScript
const chrSet: Set<string> = new Set(pat);
for (let i = 1, matchLength = 0; i < M; i++) {
      chrSet.forEach((chr) => {
        DFA[i].has(chr) || DFA[i].set(chr, getState(DFA, matchLength, chr));
      });
      matchLength = getState(DFA, matchLength, pat[i]);
    }
```

因而，当某 chr 无法匹配时对应的 `pat[i]` 时，由于 `string + chr = pat[0: matchLength] + chr`， 所以 chr 开始与`pat[matchLength]` 进行匹配， 实际上等价于 `DFA[matchLength].get(chr)`。也就是说，在匹配失败后，旧的匹配字符串被舍去， 维护的 string 作为了新的匹配字符串开头，然后开始匹配 string 后一位的 chr。在每次循环时，对所有出现的字符集合都进行一次这样的计算，并将 `DFA[matchLength].get(chr)` 的结果录入至 `DFA[i][chr]` 中。由于匹配的字符在之前的构造中已经录入，所以通过 `DFA[i].has(chr)` 判断排除这种情况。

由于此方法通过递归构造 DFA， 且 `DFA[0]` 已知， 每次for循环保证了 `DFA[i]` 的可靠性（已遍历所有可能出现的字符），所以返回的 DFA 必然有效。

由此，达到 `利用匹配字符串之间存在重复的部分` 的目的， DFA 由此构造成功。之后只需将以上几部分代码组合即可实现算法，不再赘述。

## 算法源代码

```TypeScript
type DFA = Array<Map<string, number>>;

const getState = (DFA: DFA, state: number, chr: string): number => DFA[state].get(chr) || 0;

const KMP = (pat: string): DFA => {
    const M = pat.length;
    const chrSet: Set<string> = new Set(pat);
    const DFA: DFA = [];
    for (let i = 0; i < M; i++) {
        const chr = pat[i];
        const map: Map<string, number> = new Map();
        map.set(chr, i+1);
        DFA.push(map);
    }

    for (let i = 1, matchLength = 0; i < M; i++) {
      chrSet.forEach((chr) => {
        DFA[i].has(chr) || DFA[i].set(chr, getState(DFA, matchLength, chr));
      });
      matchLength = getState(DFA, matchLength, pat[i]);
    }

    return DFA;
}

const strStr = (txt: string, pat: string): number => {
    if (pat === '') return 0;
    const DFA = KMP(pat);
    for (let matchLength = 0， i=0; i < txt.length; i++) {
        const chr = txt[i];
        matchLength = getState(DFA, matchLength, chr);
        if (matchLength === pat.length) return i - pat.length + 1;
    }
    return -1;
}
```
