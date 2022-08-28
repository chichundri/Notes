* DeadLockDemo 
```
public class DeadLockDemo {
	public void method1() {
		synchronized (String.class) {
			System.out.println("Aquired lock on String.class object");
			synchronized (Integer.class) {
				System.out.println("Aquired lock on Integer.class object");
			}
		}
	}

	public void method2() {
		synchronized (Integer.class) {
			System.out.println("Aquired lock on Integer.class object");
			synchronized (String.class) {
				System.out.println("Aquired lock on String.class object");
			}
		}
	}
}

```
* Longest consecutive sequence in array
Approach 1
```
	public int longestConsecutive1(int[] nums) {
		if (nums.length == 0) {
			return 0;
		}

		Arrays.sort(nums);

		int longestStreak = 1;
		int currentStreak = 1;

//		1,2,3,4,5,10,11
		
		for (int i = 1; i < nums.length; i++) {
			if (nums[i] != nums[i - 1]) {
				if (nums[i] == nums[i - 1] + 1) {
					currentStreak += 1;
				} else {
					longestStreak = Math.max(longestStreak, currentStreak);
					currentStreak = 1;
				}
			}
		}
		return Math.max(longestStreak, currentStreak);
	}
```  
Approach 2 

```
	public int longestConsecutive(int[] nums) {
		Set<Integer> num_set = new HashSet<Integer>();
		for (int num : nums) {
			num_set.add(num);
		}

		int longestStreak = 0;

		for (int num : num_set) {
			if (!num_set.contains(num - 1)) {
				int currentNum = num;
				int currentStreak = 1;

				while (num_set.contains(currentNum + 1)) {
					currentNum += 1;
					currentStreak += 1;
				}

				longestStreak = Math.max(longestStreak, currentStreak);
			}
		}

		return longestStreak;
	}
```





