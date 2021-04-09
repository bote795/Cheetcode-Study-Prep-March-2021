# bit manipulation

```
Given two integers dividend and divisor, divide two integers without using multiplication, division, and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero, which means losing its fractional part. For example, truncate(8.345) = 8 and truncate(-2.7335) = -2.

Note: Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. For this problem, assume that your function returns 231 − 1 when the division result overflows.
Example 1:

Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = truncate(3.33333..) = 3.
Example 2:

Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = truncate(-2.33333..) = -2.
Example 3:

Input: dividend = 0, divisor = 1
Output: 0
Example 4:

Input: dividend = 1, divisor = 1
Output: 1

```

```javascript
/**

1. We want to keep multiplying the divisor by 2 while that value is bigger than dividend.
2. If its bigger than dividend, we go back to the base divisor value and start doubling again until we cannot

Bit wise operators explained
A << B === A * Math.pow(2, B); if we do 3 << 1, we want to multiple 3 by 2^1 === 6
A >> B === Math.floor(A / Math.pow(2, B)); if we do 3 >> 1, we want to divide 3 by 2^1 while rounding down === 1

 * @param {number} dividend
 * @param {number} divisor
 * @return {number}
 */
var divide = function (dividend, divisor) {
  let min = Math.pow(-2, 31);
  let max = Math.pow(2, 31) - 1;
  let signEnd = Math.sign(dividend);
  let signSor = Math.sign(divisor);
  let dividendAb = Math.abs(dividend);
  let divisorAb = Math.abs(divisor);
  let counter = 0;
  while (dividendAb >= divisorAb) {
    let newDivisor = divisorAb;
    let count = 1;
    // while we can double, we keep doubling
    // if double exceeded half of the dividend, there's no need for further doubling
    // A >> num (number of times this A divide by 2)
    while (dividendAb >> 1 >= newDivisor) {
      // while(dividendAb >= newDivisor + newDivisor) {
      // A single left shift(<<) multiplies a binary number by 2
      // A << num (number of times A gets multipled by 2)
      newDivisor = newDivisor << 1;
      count = count << 1;
      // newDivisor += newDivisor;
      // count += count;
    }
    // else we keep adding by 1 count
    dividendAb -= newDivisor;
    counter += count;
  }
  if (signEnd === signSor && (signEnd === 1 || signEnd === -1)) {
    return Math.min(counter, max);
  }
  return Math.max(-counter, min);
};
```
