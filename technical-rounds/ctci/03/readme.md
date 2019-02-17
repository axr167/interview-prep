
### 1. Implement 3 stacks with 1 array

If stack is fixed size:

- Divide stack into n/3 parts
  - S1 = 0...n/3-1
  - S2 = n/3....2n/3-1
  - S3 = 2n/3...n

If stack is not fixed size:

- Use arraylist
- 0...9 = S1
- 10...19 = S2
- 20...29 = S3
- 30...39 = S1 and so on

