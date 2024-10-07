# Reservation-system-

import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class OnlineReservationSystem {
    private static Map<String, String> users = new HashMap<>(); // Store user credentials
    private static Map<String, Reservation> reservations = new HashMap<>(); // Store reservations
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        users.put("user1", "password1"); // Example user
        users.put("user2", "password2");

        System.out.println("Welcome to the Online Reservation System");
        if (login()) {
            while (true) {
                System.out.println("\n1. Make a Reservation");
                System.out.println("2. Cancel a Reservation");
                System.out.println("3. Exit");
                System.out.print("Choose an option: ");
                int choice = scanner.nextInt();

                switch (choice) {
                    case 1:
                        makeReservation();
                        break;
                    case 2:
                        cancelReservation();
                        break;
                    case 3:
                        System.out.println("Exiting...");
                        return;
                    default:
                        System.out.println("Invalid choice. Please try again.");
                }
            }
        }
    }

    private static boolean login() {
        System.out.print("Enter Login ID: ");
        String id = scanner.next();
        System.out.print("Enter Password: ");
        String password = scanner.next();

        if (users.containsKey(id) && users.get(id).equals(password)) {
            System.out.println("Login Successful!");
            return true;
        } else {
            System.out.println("Invalid credentials. Please try again.");
            return false;
        }
    }

    private static void makeReservation() {
        System.out.print("Enter Your Name: ");
        String name = scanner.next();
        System.out.print("Enter Train Number: ");
        String trainNumber = scanner.next();
        System.out.print("Enter Train Name: ");
        String trainName = scanner.next();
        System.out.print("Enter Class Type: ");
        String classType = scanner.next();
        System.out.print("Enter Date of Journey (YYYY-MM-DD): ");
        String dateOfJourney = scanner.next();
        System.out.print("From: ");
        String from = scanner.next();
        System.out.print("To: ");
        String to = scanner.next();

        String pnr = generatePNR(trainNumber, name); // Generate a simple PNR
        reservations.put(pnr, new Reservation(name, trainNumber, trainName, classType, dateOfJourney, from, to));
        System.out.println("Reservation successful! Your PNR is: " + pnr);
    }

    private static void cancelReservation() {
        System.out.print("Enter PNR Number to cancel: ");
        String pnr = scanner.next();

        if (reservations.containsKey(pnr)) {
            Reservation reservation = reservations.get(pnr);
            System.out.println("Reservation Details:");
            System.out.println(reservation);
            System.out.print("Do you want to confirm cancellation? (yes/no): ");
            String confirmation = scanner.next();

            if (confirmation.equalsIgnoreCase("yes")) {
                reservations.remove(pnr);
                System.out.println("Reservation cancelled successfully.");
            } else {
                System.out.println("Cancellation aborted.");
            }
        } else {
            System.out.println("Invalid PNR number.");
        }
    }

    private static String generatePNR(String trainNumber, String name) {
        return trainNumber + "_" + name.hashCode(); // Simple PNR generation logic
    }

    static class Reservation {
        String name;
        String trainNumber;
        String trainName;
        String classType;
        String dateOfJourney;
        String from;
        String to;

        Reservation(String name, String trainNumber, String trainName, String classType, String dateOfJourney, String from, String to) {
            this.name = name;
            this.trainNumber = trainNumber;
            this.trainName = trainName;
            this.classType = classType;
            this.dateOfJourney = dateOfJourney;
            this.from = from;
            this.to = to;
        }

        @Override
        public String toString() {
            return "Name: " + name + "\nTrain Number: " + trainNumber + "\nTrain Name: " + trainName +
                    "\nClass Type: " + classType + "\nDate of Journey: " + dateOfJourney +
                    "\nFrom: " + from + "\nTo: " + to;
        }
    }
}
