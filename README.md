# number_properties

package numbers;

import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);

        System.out.println("Welcome to Amazing Numbers!");

        System.out.println("Supported requests:");
        System.out.println("- enter a natural number to know its properties;");
        System.out.println("- enter two natural numbers to obtain the properties of the list:");
        System.out.println("  * the first parameter represents a starting number;");
        System.out.println("  * the second parameter shows how many consecutive numbers are to be printed;");
        System.out.println("- two natural numbers and properties to search for;");
        System.out.println("- a property preceded by minus must not be present in numbers;");
        System.out.println("- separate the parameters with one space;");
        System.out.println("- enter 0 to exit.");

        long number;
        int target;

        UI:
        while (true) {

            System.out.println("Enter a request:");

            String request = scanner.nextLine();

            String[] requestArray = request.split(" ");

            switch (requestArray.length) {

                case 1:
                    try {
                        number = Long.parseLong(requestArray[0]);
                        if (number == 0) {
                            break UI;
                        } else if (number < 0) {
                            System.out.println("The first parameter should be a natural number or zero.");
                        } else {
                            displayProperties(number);
                        }
                    } catch (NumberFormatException e) {
                        System.out.println("The first parameter should be a natural number or zero.");
                    }
                    break;
                case 2:
                    try {
                        number = Long.parseLong(requestArray[0]);
                        try {
                            target = Integer.parseInt(requestArray[1]);
                            if (number == 0) {
                                break UI;
                            } else if (number < 0) {
                                System.out.println("The first parameter should be a natural number or zero.");
                            } else if (target < 0) {
                                System.out.println("The second parameter should be a natural number.");
                            } else {
                                List<Long> numbers = new ArrayList<>();
                                for (int i = 0; i < target; i++) {
                                    numbers.add(number++);
                                }
                                displayProperties(numbers);
                            }
                        } catch (NumberFormatException e) {
                            System.out.println("The second parameter should be a natural number.");
                        }
                    } catch (NumberFormatException e) {
                        System.out.println("The first parameter should be a natural number or zero.");
                    }
                    break;
                case 3:
                    try {
                        number = Long.parseLong(requestArray[0]);
                        try {
                            target = Integer.parseInt(requestArray[1]);
                            String property = requestArray[2];
                            if (number == 0) {
                                break UI;
                            } else if (number < 0) {
                                System.out.println("The first parameter should be a natural number or zero.");
                            } else if (target < 0) {
                                System.out.println("The second parameter should be a natural number.");
                            } else if (!checkValidProperty(property)) {
                                System.out.println("The property " + property + " is wrong.");
                                System.out.println("Available properties: [EVEN, ODD, BUZZ, DUCK, PALINDROMIC, GAPFUL, SPY, SQUARE, SUNNY, JUMPING, HAPPY, SAD]");
                            } else {
                                List<Long> numbers = new ArrayList<>();
                                int count = 0;
                                while (count < target) {
                                    if (hasMinus(property.toUpperCase(), number)) {
                                        numbers.add(number);
                                        count++;
                                    }
                                    number++;
                                }
                                displayProperties(numbers);
                            }
                        } catch (NumberFormatException e) {
                            System.out.println("The second parameter should be a natural number.");
                        }
                    } catch (NumberFormatException e) {
                        System.out.println("The first parameter should be a natural number or zero.");
                    }
                    break;
                case 4:
                case 5:
                case 6:
                case 7:
                case 8:
                case 9:
                case 10:
                case 11:
                case 12:
                    try {
                        number = Long.parseLong(requestArray[0]);
                        try {
                            target = Integer.parseInt(requestArray[1]);
                            if (number == 0) {
                                break UI;
                            } else if (number < 0) {
                                System.out.println("The first parameter should be a natural number or zero.");
                            } else if (target < 0) {
                                System.out.println("The second parameter should be a natural number.");
                            } else if (!checkValidProperties(requestArray)) {
                                System.out.println("Available properties: [EVEN, ODD, BUZZ, DUCK, PALINDROMIC, GAPFUL, SPY, SQUARE, SUNNY, JUMPING, HAPPY, SAD]");
                            } else if (!checkExclusiveProperties(requestArray)) {
                                System.out.println("There are no numbers with these properties.");
                            } else {
                                List<Long> numbers = new ArrayList<>();
                                List<Long> negativeNumbers = new ArrayList<>();
                                String[] stringArray = Arrays.copyOfRange(requestArray, 2, requestArray.length);

                                int count = 0;
                                while (count < target) {
                                    boolean addToList = true;
                                    for (int i = 0; i < stringArray.length; i++) {

                                        if (!hasMinus(stringArray[i].toUpperCase(), number)){
                                            addToList = false;
                                            break;

                                        }

                                    }
                                    if (addToList) {
                                        numbers.add(number);
                                        count++;
                                    }


                                    number++;
                                }
                                displayProperties(numbers);
                            }
                        } catch (NumberFormatException e) {
                            System.out.println("The second parameter should be a natural number.");
                        }
                    } catch (NumberFormatException e) {
                        System.out.println("The first parameter should be a natural number or zero.");
                    }
            }

        }

        System.out.println("Goodbye!");

    }

    static boolean checkParity(long num) {
        return num % 2 == 0;
    }

    static boolean checkDivisible(long num) {
        return num % 7 == 0;
    }

    static boolean checkLastDigit(long num) {
        long temp = num % 10;
        return temp == 7;
    }

    static boolean checkBuzz(long num) {
        return checkDivisible(num) || checkLastDigit(num);
    }

    static boolean checkDuck(long num) {
        String str = String.valueOf(num);
        for (int i = 1; i < str.length(); i++) {
            if (str.charAt(i) == '0') {
                return true;
            }
        }
        return false;
    }

    static boolean checkPalindromic(long num) {
        String str = String.valueOf(num);
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) != str.charAt(str.length() - i - 1)) {
                return false;
            }
        }
        return true;
    }

    static boolean checkGapful(long num) {
        String str = String.valueOf(num);
        String[] arr = str.split("");
        if (arr.length < 3) {
            return false;
        } else {
            long num1 = Long.parseLong(arr[0]);
            long num2 = Long.parseLong(arr[arr.length - 1]);
            long div = Long.parseLong(num1 + "" + num2);
            return num % div == 0;
        }
    }

    static boolean checkSpy(long num) {
        String str = String.valueOf(num);
        String[] arr = str.split("");
        long sum = 0;
        long product = 1;
        for (String s : arr) {
            sum += Long.parseLong(s);
            product *= Long.parseLong(s);
        }
        return sum == product;
    }

    static boolean checkSquare(long num) {
        return Math.sqrt(num) == (long) Math.sqrt(num);
    }

    static boolean checkSunny(long num) {
        return Math.sqrt(num + 1) == (long) Math.sqrt(num + 1);
    }
    static boolean checkHappy(long num) {
        if (num == 1 || num == 7)
            return true;
        long sum = num, x = num;

        while (sum > 9) {
            sum = 0;

            while (x > 0) {
                long d = x % 10;
                sum += d * d;
                x /= 10;
            }
            if (sum == 1)
                return true;
            x = sum;
        }
        if (sum == 7)
            return true;
        return false;
    }
    static boolean checkJumping(long num) {
        String str = String.valueOf(num);
        String[] arr = str.split("");
        if (arr.length == 1) {
            return true;
        }
        boolean jumping = true;
        for (int i = 1; i < arr.length; i++) {
            if (Math.abs(Long.parseLong(arr[i]) - Long.parseLong(arr[i - 1])) != 1) {
                jumping = false;
                break;
            }
        }
        return jumping;

    }
    static boolean hasMinus(String property, long num) {
        var isNegative = property.charAt(0) == '-';
        return isNegative ? !checkProperty(property.substring(1), num) : checkProperty(property, num);
    }


    static boolean checkProperty(String property, long num) {
        boolean result = false;
        switch (property) {
            case "EVEN":
                result = checkParity(num);
                break;
            case "ODD":
                result = !checkParity(num);
                break;
            case "BUZZ":
                result = checkBuzz(num);
                break;
            case "DUCK":
                result = checkDuck(num);
                break;
            case "GAPFUL":
                result = checkGapful(num);
                break;
            case "JUMPING":
                result = checkJumping(num);
                break;
            case "PALINDROMIC":
                result = checkPalindromic(num);
                break;
            case "SPY":
                result = checkSpy(num);
                break;
            case "SQUARE":
                result = checkSquare(num);
                break;
            case "SUNNY":
                result = checkSunny(num);
                break;
            case "HAPPY":
                result = checkHappy(num);
                break;
            case "SAD":
                result = !checkHappy(num);
                break;
        
        }
        return result;
    }


    static void displayProperties(long num) {
        System.out.println("Properties of " + num);
        System.out.println("buzz: " + checkBuzz(num));
        System.out.println("duck: " + checkDuck(num));
        System.out.println("palindromic: " + checkPalindromic(num));
        System.out.println("gapful: " + checkGapful(num));
        System.out.println("spy: " + checkSpy(num));
        System.out.println("square: " + checkSquare(num));
        System.out.println("sunny: " + checkSunny(num));
        System.out.println("jumping: " + checkJumping(num));
        System.out.println("even: " + checkParity(num));
        System.out.println("odd: " + !checkParity(num));
        System.out.println("happy: " + checkHappy(num));
        System.out.println("sad: " + !checkHappy(num));
    }





    static void displayProperties(List<Long> numbers) {
        for (Long num : numbers) {
            String buzz = checkBuzz(num) ? "buzz, " : "";
            String duck = checkDuck(num) ? "duck, " : "";
            String palindromic = checkPalindromic(num) ? "palindromic, " : "";
            String gapful = checkGapful(num) ? "gapful, " : "";
            String spy = checkSpy(num) ? "spy, " : "";
            String square = checkSquare(num) ? "square, " : "";
            String sunny = checkSunny(num) ? "sunny, " : "";
            String jumping = checkJumping(num) ? "jumping, " : "";
            String even = checkParity(num) ? "even, " : "";
            String odd = !checkParity(num) ? "odd" : "";
            String happy = checkHappy(num) ? "happy, " : "";
            String sad = !checkHappy(num) ? "sad, " : "";

            System.out.println(num + " is " + buzz + duck + palindromic + gapful + spy + square + sunny + jumping + happy + sad + even + odd);
        }
    }

    static boolean checkValidProperty(String property) {
        String[] properties = {"EVEN", "ODD", "BUZZ", "DUCK", "PALINDROMIC", "GAPFUL", "SPY", "SQUARE", "SUNNY", "JUMPING", "HAPPY", "SAD",
                "-EVEN", "-ODD", "-BUZZ", "-DUCK", "-PALINDROMIC", "-GAPFUL", "-SPY", "-SQUARE", "-SUNNY", "-JUMPING", "-HAPPY", "-SAD"};
        boolean found = false;
        for (String str : properties) {
            if (str.equalsIgnoreCase(property)) {
                found = true;
                break;
            }
        }
        return found;
    }

    static boolean checkValidProperties(String[] requestArray) {
        List<String> invalidProperties = new ArrayList<>();
        for (int i = 2; i < requestArray.length; i++) {
            if (!checkValidProperty(requestArray[i])) {
                invalidProperties.add(requestArray[i]);
            }
        }
        if (invalidProperties.size() == 1) {
            System.out.println("The property " + invalidProperties.get(0) + " is wrong.");
        } else if (invalidProperties.size() > 1) {
            System.out.println("The properties " + invalidProperties + " are wrong.");
        }
        return invalidProperties.size() == 0;
    }

    static boolean checkExclusiveProperties(String[] requestArray) {
        List<String> invalidProperties = new ArrayList<>();
        if (Arrays.asList(requestArray).contains("even") && Arrays.asList(requestArray).contains("odd")) {
            invalidProperties.add("even");
            invalidProperties.add("odd");
            System.out.println("The request contains mutually exclusive properties: even, odd");
        } else if (Arrays.asList(requestArray).contains("duck") && Arrays.asList(requestArray).contains("spy")) {
            invalidProperties.add("duck");
            invalidProperties.add("spy");
            System.out.println("The request contains mutually exclusive properties: duck, spy");
        } else if (Arrays.asList(requestArray).contains("sunny") && Arrays.asList(requestArray).contains("square")) {
            invalidProperties.add("sunny");
            invalidProperties.add("square");
            System.out.println("The request contains mutually exclusive properties: sunny, square");
        } else if (Arrays.asList(requestArray).contains("-odd") && Arrays.asList(requestArray).contains("-even")) {
            invalidProperties.add("even");
            invalidProperties.add("odd");
            System.out.println("The request contains mutually exclusive properties: even, odd");
        } else if (Arrays.asList(requestArray).contains("-duck") && Arrays.asList(requestArray).contains("-spy")) {
            invalidProperties.add("duck");
            invalidProperties.add("spy");
            System.out.println("The request contains mutually exclusive properties: duck, spy");
        } else if (Arrays.asList(requestArray).contains("-sunny") && Arrays.asList(requestArray).contains("-square")) {
            invalidProperties.add("sunny");
            invalidProperties.add("square");
            System.out.println("The request contains mutually exclusive properties: sunny, square");
        } else if (Arrays.asList(requestArray).contains("-odd") && Arrays.asList(requestArray).contains("odd")) {
            invalidProperties.add("-odd");
            invalidProperties.add("odd");
            System.out.println("The request contains mutually exclusive properties: -odd, odd");
        } else if (Arrays.asList(requestArray).contains("-even") && Arrays.asList(requestArray).contains("even")) {
            invalidProperties.add("even");
            invalidProperties.add("-even");
            System.out.println("The request contains mutually exclusive properties: even, -even");
        } else if (Arrays.asList(requestArray).contains("-duck") && Arrays.asList(requestArray).contains("duck")) {
            invalidProperties.add("duck");
            invalidProperties.add("-duck");
            System.out.println("The request contains mutually exclusive properties: duck, -duck");
        }

        return invalidProperties.size() == 0;
    }

}
