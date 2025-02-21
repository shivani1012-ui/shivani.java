public class SecretSharing {

    // Function to convert the value from a given base to decimal
    public static long convertToDecimal(String value, int base) {
        long decimalValue = 0;
        int len = value.length();
        for (int i = 0; i < len; i++) {
            char digit = value.charAt(i);
            int digitValue = (digit >= '0' && digit <= '9') ? digit - '0' : (digit - 'a' + 10);
            decimalValue += digitValue * Math.pow(base, len - i - 1);
        }
        return decimalValue;
    }

    // Function to calculate Lagrange interpolation for the given points
    public static long lagrangeInterpolation(int[][] shares, int n, int xValue) {
        double total = 0.0;
        for (int i = 0; i < n; i++) {
            double term = shares[i][1];  // yi
            for (int j = 0; j < n; j++) {
                if (i != j) {
                    term *= (double) (xValue - shares[j][0]) / (shares[i][0] - shares[j][0]);
                }
            }
            total += term;
        }
        return (long) total;
    }

    // Function to reconstruct the secret using Lagrange interpolation at x = 0
    public static long reconstructSecret(int n, int k, int[][] shares) {
        return lagrangeInterpolation(shares, k, 0);
    }

    public static void main(String[] args) {
        // Test Case 1
        int[][] shares1 = {
            {1, (int) convertToDecimal("4", 10)},     // (1, 4)
            {2, (int) convertToDecimal("111", 2)},    // (2, 7)
            {3, (int) convertToDecimal("12", 10)},    // (3, 12)
            {6, (int) convertToDecimal("213", 4)}     // (6, 39)
        };

        long secret1 = reconstructSecret(4, 3, shares1);
        System.out.println("Secret key from test case 1: " + secret1);

        // Test Case 2
        int[][] shares2 = {
            {1, (int) convertToDecimal("420020006424065463", 7)},
            {2, (int) convertToDecimal("10511630252064643035", 7)},
            {3, (int) convertToDecimal("101010101001100101011100000001000111010010111101100100010", 2)},
            {4, (int) convertToDecimal("31261003022226126015", 8)},
            {5, (int) convertToDecimal("2564201006101516132035", 7)},
            {6, (int) convertToDecimal("a3c97ed550c69484", 15)},
            {7, (int) convertToDecimal("134b08c8739552a734", 13)},
            {8, (int) convertToDecimal("23600283241050447333", 10)},
            {9, (int) convertToDecimal("375870320616068547135", 9)},
            {10, (int) convertToDecimal("30140555423010311322515333", 6)}
        };

        long secret2 = reconstructSecret(10, 7, shares2);
        System.out.println("Secret key from test case 2: " + secret2);
    }
}
