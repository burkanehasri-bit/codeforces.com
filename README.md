# codeforces.com

A. Theatre Square
------------------
Theatre Square in the capital city of Berland has a rectangular shape with the size n × m meters. On the occasion of the city's anniversary, a decision was taken to pave the Square with square granite flagstones. Each flagstone is of the size a × a.

What is the least number of flagstones needed to pave the Square? It's allowed to cover the surface larger than the Theatre Square, but the Square has to be covered. It's not allowed to break the flagstones. The sides of flagstones should be parallel to the sides of the Square.

Input
The input contains three positive integer numbers in the first line: n,  m and a (1 ≤  n, m, a ≤ 109).

Output
Write the needed number of flagstones.

Examples
Input

6 6 4

Output

4

CODE
----
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        long n = sc.nextLong();
        long m = sc.nextLong();
        long a = sc.nextLong();

        long rows = (n + a - 1) / a; // ceil(n/a)
        long cols = (m + a - 1) / a; // ceil(m/a)

        System.out.println(rows * cols);
    }
}

B. Spreadsheets
----------------
In the popular spreadsheets systems (for example, in Excel) the following numeration of columns is used. The first column has number A, the second — number B, etc. till column 26 that is marked by Z. Then there are two-letter numbers: column 27 has number AA, 28 — AB, column 52 is marked by AZ. After ZZ there follow three-letter numbers, etc.

The rows are marked by integer numbers starting with 1. The cell name is the concatenation of the column and the row numbers. For example, BC23 is the name for the cell that is in column 55, row 23.

Sometimes another numeration system is used: RXCY, where X and Y are integer numbers, showing the column and the row numbers respectfully. For instance, R23C55 is the cell from the previous example.

Your task is to write a program that reads the given sequence of cell coordinates and produce each item written according to the rules of another numeration system.

Input
The first line of the input contains integer number n (1 ≤ n ≤ 10^5), the number of coordinates in the test. Then there follow n lines, each of them contains coordinates. All the coordinates are correct, there are no cells with the column and/or the row numbers larger than 10^6 .

Output
Write n lines, each line should contain a cell coordinates in the other numeration system.

Examples
Input

2

R23C55

BC23

Output

BC23

R23C55

CODE
----
import java.io.*;
import java.util.regex.*;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());

        Pattern p = Pattern.compile("^R\\d+C\\d+$");

        StringBuilder ans = new StringBuilder();

        for (int i = 0; i < n; i++) {
            String s = br.readLine();

            if (p.matcher(s).matches()) {
                // RXCY -> Excel
                int cPos = s.indexOf('C');
                int row = Integer.parseInt(s.substring(1, cPos));
                int col = Integer.parseInt(s.substring(cPos + 1));

                StringBuilder colName = new StringBuilder();

                while (col > 0) {
                    col--;
                    colName.append((char) ('A' + (col % 26)));
                    col /= 26;
                }

                ans.append(colName.reverse()).append(row).append("\n");

            } else {
                // Excel -> RXCY
                int idx = 0;
                while (idx < s.length() && Character.isLetter(s.charAt(idx))) {
                    idx++;
                }

                String letters = s.substring(0, idx);
                String row = s.substring(idx);

                long col = 0;
                for (char ch : letters.toCharArray()) {
                    col = col * 26 + (ch - 'A' + 1);
                }

                ans.append("R").append(row).append("C").append(col).append("\n");
            }
        }

        System.out.print(ans);
    }
}



A. Winner
--------

The winner of the card game popular in Berland "Berlogging" is determined according to the following rules. If at the end of the game there is only one player with the maximum number of points, he is the winner. The situation becomes more difficult if the number of such players is more than one. During each round a player gains or loses a particular number of points. In the course of the game the number of points is registered in the line "name score", where name is a player's name, and score is the number of points gained in this round, which is an integer number. If score is negative, this means that the player has lost in the round. So, if two or more players have the maximum number of points (say, it equals to m) at the end of the game, than wins the one of them who scored at least m points first. Initially each player has 0 points. It's guaranteed that at the end of the game at least one player has a positive number of points.

Input
The first line contains an integer number n (1  ≤  n  ≤  1000), n is the number of rounds played. Then follow n lines, containing the information about the rounds in "name score" format in chronological order, where name is a string of lower-case Latin letters with the length from 1 to 32, and score is an integer number between -1000 and 1000, inclusive.

Output
Print the name of the winner.

Examples

Input

3

mike 3

andrew 5

mike 2

Output

andrew

Input

3

andrew 3

andrew 2

mike 5

Output

andrew

CODE:

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine().trim());

        String[] names = new String[n];
        int[] scores = new int[n];
        Map<String, Integer> finalScores = new HashMap<>();

        // Pass 1: Parse input and find the final scores of all players
        for (int i = 0; i < n; i++) {
            String[] parts = br.readLine().split(" ");
            names[i] = parts[0];
            scores[i] = Integer.parseInt(parts[1]);
            
            finalScores.put(names[i], finalScores.getOrDefault(names[i], 0) + scores[i]);
        }

        // Find the maximum final score among all players
        int maxScore = Integer.MIN_VALUE;
        for (int score : finalScores.values()) {
            if (score > maxScore) {
                maxScore = score;
            }
        }

        // Pass 2: Re-simulate to find who reached >= maxScore first
        Map<String, Integer> currentScores = new HashMap<>();
        for (int i = 0; i < n; i++) {
            currentScores.put(names[i], currentScores.getOrDefault(names[i], 0) + scores[i]);
            
            // Candidate must reach >= maxScore AND must finish the game with exactly maxScore
            if (currentScores.get(names[i]) >= maxScore && finalScores.get(names[i]) == maxScore) {
                System.out.println(names[i]);
                return;
            }
        }
    }
}

C. Ancient Berland Circus
-------------------------

Nowadays all circuses in Berland have a round arena with diameter 13 meters, but in the past things were different.

In Ancient Berland arenas in circuses were shaped as a regular (equiangular) polygon, the size and the number of angles could vary from one circus to another. In each corner of the arena there was a special pillar, and the rope strung between the pillars marked the arena edges.

Recently the scientists from Berland have discovered the remains of the ancient circus arena. They found only three pillars, the others were destroyed by the time.

You are given the coordinates of these three pillars. Find out what is the smallest area that the arena could have.

Input
The input file consists of three lines, each of them contains a pair of numbers –– coordinates of the pillar. Any coordinate doesn't exceed 1000 by absolute value, and is given with at most six digits after decimal point.

Output
Output the smallest possible area of the ancient arena. This number should be accurate to at least 6 digits after the decimal point. It's guaranteed that the number of angles in the optimal polygon is not larger than 100.

Examples


Input

0.000000 0.000000

1.000000 1.000000

0.000000 1.000000

Output

1.00000000

CODE:

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        double[] x = new double[3];
        double[] y = new double[3];
        
        for (int i = 0; i < 3; i++) {
            String[] tokens = br.readLine().trim().split("\\s+");
            x[i] = Double.parseDouble(tokens[0]);
            y[i] = Double.parseDouble(tokens[1]);
        }
        
        // Calculate side lengths
        double a = Math.hypot(x[0] - x[1], y[0] - y[1]);
        double b = Math.hypot(x[1] - x[2], y[1] - y[2]);
        double c = Math.hypot(x[2] - x[0], y[2] - y[0]);
        
        // Calculate area of the triangle via Heron's formula
        double s = (a + b + c) / 2.0;
        double areaTri = Math.sqrt(s * (s - a) * (s - b) * (s - c));
        
        // Calculate circumradius R
        double r = (a * b * c) / (4.0 * areaTri);
        
        // Calculate the central angles subtended by the sides
        double alpha = Math.acos(clamp(1.0 - (a * a) / (2.0 * r * r)));
        double beta = Math.acos(clamp(1.0 - (b * b) / (2.0 * r * r)));
        double gamma = 2.0 * Math.PI - (alpha + beta);
        
        // Find the absolute greatest common divisor among the central angles
        double angleGcd = gcd(alpha, gcd(beta, gamma));
        
        // Number of sides n
        double n = (2.0 * Math.PI) / angleGcd;
        
        // Total area of the minimal regular polygon
        double totalArea = 0.5 * n * r * r * Math.sin(2.0 * Math.PI / n);
        
        System.out.printf("%.8f\n", totalArea);
    }
    
    // Floating point GCD using Euclidean algorithm with tolerance
    private static double gcd(double a, double b) {
        while (Math.abs(b) > 1e-4) {
            double temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
    
    // Clamp to bypass floating point imprecisions outside [-1, 1] range for acos
    private static double clamp(double val) {
        return Math.max(-1.0, Math.min(1.0, val));
    }
}

