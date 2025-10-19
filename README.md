# recursiveSicuence
package org.sorokin;

import java.util.ArrayList;
import java.util.List;

public class Main {

    public static int count = 0;

    public static void main(String[] args) {
        compare(10);
//        compare(2);
//        compare(5);
//        compare(15);
    }

    public static void compare(int day) {
        System.out.println("=== Day " + day + " ===");
        int[] startNumbers = {21, 1, 20, 23};
        long start1 = System.nanoTime();
        int iterative = chooseHobbyIterative(startNumbers, day);
        long end1 = System.nanoTime();
        long start2 = System.nanoTime();
        int recursive = chooseHobbyRecursive(startNumbers, day);
        long end2 = System.nanoTime();
        long start3 = System.nanoTime();
        int recursive2 = chooseHobbyRecursivePre(startNumbers, day);
        long end3 = System.nanoTime();
        System.out.println("Time: " + (end1 - start1) + " | " + (end2 - start2) + " | " + (end3 - start3));
        System.out.println("Iterative = " + iterative + " | Recursive = " + recursive + " | Recursive2 = " + recursive2);
        System.out.println();
    }

    public static int chooseHobbyRecursive(int[] startNumbers, int day) {
        int index = startNumbers.length - 1 + day;
        if (index < startNumbers.length) {
            return startNumbers[index];
        }
        int[] memo = new int[startNumbers.length + day];

        for (int i = 0; i < startNumbers.length; i++) {
            if (memo[i] == 0) {
                memo[i] = startNumbers[i];
            }
        }
        return compute(memo, index);
    }

    private static int compute(int[] memo, int index) {
        //System.out.println(count++);
        if (memo[index] != 0) {
            return memo[index];
        }
        int prev = compute(memo, index - 1);
        //int prePrePrev = compute(memo, index - 3);
        int prePrePrev = memo[index-3];
        memo[index] = (prev * prePrePrev) % 10 + 1;
        return memo[index];
    }

    public static int chooseHobbyRecursivePre(int[] startNumbers, int day) {
        int index = startNumbers.length - 1 + day;

        if (index < startNumbers.length - 1) {
            return startNumbers[index];
        }
        int prev = chooseHobbyRecursivePre(startNumbers, day - 1);
        int prePrePrev = chooseHobbyRecursivePre(startNumbers, day - 3);

        return (prev * prePrePrev) % 10 + 1;
    }

    public static int chooseHobbyIterative(int[] startNumbers, int day) {
        List<Integer> numbers = new ArrayList<>();

        numbers.add(startNumbers[0]);
        numbers.add(startNumbers[1]);
        numbers.add(startNumbers[2]);
        numbers.add(startNumbers[3]);

        for (int d = 0; d < day; d++) {
            int index = d + 4; // индексы дней в массиве сдвинуты на 4
            int prev = numbers.get(index - 1); // предыдущее значение
            int prePrePrev = numbers.get(index - 3); // пре-пре-предыдущее значение
            numbers.add((prev * prePrePrev) % 10 + 1);
        }

        return numbers.get(numbers.size() - 1);
    }
}
