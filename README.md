# dive-into-offer

In order to get a job offer, I'm now training myselft through [LeetCode](https://leetcode.com), and I'd like to share my code here.

This project won't stop until I have got a job offer.

## Table of Contents

- [413 Arithmetic Slices](#413)
- [627 Swap Salary](#627)
- [717 1-bit and 2-bit Characters](#717)
- [748 Shortest Completing Word](#748)

## 413

A sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, these are arithmetic sequence:

> 1, 3, 5, 7, 9
> 7, 7, 7, 7
> 3, -1, -5, -9

The following sequence is not arithmetic.

> 1, 1, 2, 5, 7

A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.

A slice (P, Q) of array A is called arithmetic if the sequence:
A[P], A[p + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.(And this means the sequence is at least 3 elements)

The function should return the number of arithmetic slices in the array A.

Example:

> A = [1, 2, 3, 4]
>
> return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.

Solution:

Find the law from the numbers:

> 1, 2, 3 => 1
> 1, 2, 3, 4 => 3 = 1 + 2
> 1, 2, 3, 4, 5 => 6 = 3 + 3 = 1 + 2 + 3
> 1, 2, 3, 4, 5, 6 => 10 = 6 + 4 = 1 + 2 + 3 + 4
> 1, 2, 3, 4, 5, 6, 7 => 15 = 10 + 5 = 1 + 2 + 3 + 4 + 5

And apply the law to another sequences to verify it.

## 627

Given a table `salary`, such as the one below, that has m=male and f=female values. Swap all f and m values (i.e., change all f values to m and vice versa) with a single update query and no intermediate temp table.

For example:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

After running your query, the above salary table should have the following rows:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

Solution:

```sql
-- Solution 1
UPDATE `salary` SET sex = (
    CASE sex
        WHEN 'm' THEN 'f'
        ELSE 'm'
    END
)

-- Solution 2
UPDATE salary SET sex = CHAR(ASCII(sex) ^ 11);
-- OR
UPDATE salary SET sex = CHAR(ASCII('f') ^ ASCII('m') ^ ASCII(sex));
-- Explanation: 
-- ASCII('f') = 109
-- ASCII('m') = 102
-- 109 ^ 102 = 11
-- 11 ^ 109 = 102
-- 11 ^ 102 = 109
```

## 717

We have two special characters. The first character can be represented by one bit `0`. The second character can be represented by two bits (`10` or `11`).

Now given a string represented by several bits. Return whether the last character must be a one-bit character or not. The given string will always end with a zero.

Example 1:

> Input: 
> bits = [1, 0, 0]
> Output: True
> Explanation: The only way to decode it is two-bit character and one-bit character. So the last character is one-bit character.

Example 2:

> Input: 
> bits = [1, 1, 1, 0]
> Output: False
> Explanation: The only way to decode it is two-bit character and two-bit character. So the last character is NOT one-bit character.

Note:

- 1 <= len(bits) <= 1000.
- bits[i] is always 0 or 1.

Solution:

通过索引 i 的跳转，遇到 1 跳两步，遇到 0 跳一步。当 while 循环结束的时候，**通过 i 的最终位置来判断**，如果 i 的位置刚好是 bits 数组最后一个元素，则 true；如果 i 的位置跳过了最后一个元素，则 false。

## 748

Find the minimum length word from a given dictionary `words`, which has all the letters from the string `licensePlate`. Such a word is said to complete the given string `licensePlate`.

Here, for letters we ignore case. For example, `"P"` on the licensePlate still matches `"p"` on the word.

It is guaranteed an answer exists. If there are multiple answers, return the one that occurs first in the array.

The license plate might have the same letter occurring multiple times. For example, given a `licensePlate` of `"PP"`, the word `"pair"` does not complete the `licensePlate`, but the word `"supper"` does.

Example 1:

> Input: licensePlate = "1s3 PSt", words = ["step", "steps", "stripe", "stepple"]
> Output: "steps"
> Explanation: The smallest length word that contains the letters "S", "P", "S", and "T".
Note that the answer is not "step", because the letter "s" must occur in the word twice.
Also note that we ignored case for the purposes of comparing whether a letter exists in the word.

Example 2:

> Input: licensePlate = "1s3 456", words = ["looks", "pest", "stew", "show"]
> Output: "pest"
> Explanation: There are 3 smallest length words that contains the letters "s".
We return the one that occurred first.

Note:

1. `licensePlate` will be a string with length in range `[1, 7]`.
1. `licensePlate` will contain digits, spaces, or letters (uppercase or lowercase).
1. `words` will have a length in the range `[10, 1000]`.
1. Every `words[i]` will consist of lowercase letters, and have length in range `[1, 15]`.

Solution:

使用 Map 存储 word 和 licensePlate 中各个字符的数量，然后通过比较判断 word 是否合法。之后再寻找合法的 word 中最短的。