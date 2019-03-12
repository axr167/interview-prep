
### Min/Max xor pair in array

// Need help

### Number of 1 bits

Write a function that takes an unsigned integer and return the number of '1' bits it has

    public int hammingWeight(int n) {
        int count = 0;
        while(n!=0) {
            int res = n&1;
            if(res == 1) {
                count++;
            }
            n = n>>>1;
        }
        return count;
    }

### Single number

    public int singleNumber(int[] nums) {
        int x = nums[0];
        for(int i=1; i<nums.length; i++)
            x = x^nums[i];
        return x;
    }
