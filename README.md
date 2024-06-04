//TASK-1   ONLINE RESERVATION SYSTEM

import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.UUID;


public class OnlineReservationSystem extends JFrame {
    private JTextField userField;
    private JPasswordField passField;
    private JButton loginButton;
    private JTextField nameField;
    private JTextField trainNumberField;
    private JTextField trainNameField;
    private JComboBox<String> classTypeBox;
    private JTextField dateField;
    private JTextField fromField;
    private JTextField toField;
    private JButton insertButton;
    private JTextField pnrField;
    private JButton cancelButton;
    private JButton openCancelFormButton;
    private ArrayList<Reservation> reservations;

    public OnlineReservationSystem() {
        reservations = new ArrayList<>();
        initLoginForm();
    }


    private void initLoginForm() {
        setTitle("Login Form");
        setSize(300, 150);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel();
        panel.setLayout(null);

        JLabel userLabel = new JLabel("User:");
        userLabel.setBounds(10, 10, 80, 25);
        panel.add(userLabel);

        userField = new JTextField(20);
        userField.setBounds(100, 10, 160, 25);
        panel.add(userField);

        JLabel passLabel = new JLabel("Password:");
        passLabel.setBounds(10, 40, 80, 25);
        panel.add(passLabel);

        passField = new JPasswordField(20);
        passField.setBounds(100, 40, 160, 25);
        panel.add(passField);

        loginButton = new JButton("Login");
        loginButton.setBounds(100, 80, 80, 25);
        panel.add(loginButton);

        add(panel);
        setVisible(true);

        loginButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String user = userField.getText();
                String pass = new String(passField.getPassword());

                // For demo purposes, we'll accept any non-empty credentials
                if (user.isEmpty() || pass.isEmpty()) {
                    JOptionPane.showMessageDialog(null, "Please enter both username and password");
                } else {
                    initReservationForm();
                }
            }
        });
    }


    private void initReservationForm() {
        getContentPane().removeAll();
        repaint();
        setTitle("Reservation Form");
        setSize(400, 400);

        JPanel panel = new JPanel();
        panel.setLayout(null);

        JLabel nameLabel = new JLabel("Name:");
        nameLabel.setBounds(10, 10, 80, 25);
        panel.add(nameLabel);

        nameField = new JTextField(20);
        nameField.setBounds(100, 10, 160, 25);
        panel.add(nameField);

        JLabel trainNumberLabel = new JLabel("Train Number:");
        trainNumberLabel.setBounds(10, 40, 100, 25);
        panel.add(trainNumberLabel);

        trainNumberField = new JTextField(20);
        trainNumberField.setBounds(120, 40, 140, 25);
        panel.add(trainNumberField);

        JLabel trainNameLabel = new JLabel("Train Name:");
        trainNameLabel.setBounds(10, 70, 100, 25);
        panel.add(trainNameLabel);

        trainNameField = new JTextField(20);
        trainNameField.setBounds(120, 70, 140, 25);
        panel.add(trainNameField);

        JLabel classTypeLabel = new JLabel("Class Type:");
        classTypeLabel.setBounds(10, 100, 80, 25);
        panel.add(classTypeLabel);

        classTypeBox = new JComboBox<>(new String[] { "First Class", "Second Class", "Third Class" });
        classTypeBox.setBounds(100, 100, 160, 25);
        panel.add(classTypeBox);

        JLabel dateLabel = new JLabel("Date of Journey:");
        dateLabel.setBounds(10, 130, 120, 25);
        panel.add(dateLabel);

        dateField = new JTextField(20);
        dateField.setBounds(140, 130, 120, 25);
        panel.add(dateField);

        JLabel fromLabel = new JLabel("From:");
        fromLabel.setBounds(10, 160, 80, 25);
        panel.add(fromLabel);

        fromField = new JTextField(20);
        fromField.setBounds(100, 160, 160, 25);
        panel.add(fromField);

        JLabel toLabel = new JLabel("To:");
        toLabel.setBounds(10, 190, 80, 25);
        panel.add(toLabel);

        toField = new JTextField(20);
        toField.setBounds(100, 190, 160, 25);
        panel.add(toField);

        insertButton = new JButton("Insert");
        insertButton.setBounds(100, 230, 80, 25);
        panel.add(insertButton);

        openCancelFormButton = new JButton("Cancel Reservation");
        openCancelFormButton.setBounds(200, 230, 160, 25);
        panel.add(openCancelFormButton);

        add(panel);
        setVisible(true);

        insertButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String name = nameField.getText();
                String trainNumber = trainNumberField.getText();
                String trainName = trainNameField.getText();
                String classType = (String) classTypeBox.getSelectedItem();
                String date = dateField.getText();
                String from = fromField.getText();
                String to = toField.getText();

                Reservation reservation = new Reservation(name, trainNumber, trainName, classType, date, from, to);
                reservations.add(reservation);

                JOptionPane.showMessageDialog(null, "Reservation added successfully!");
            }
        });

        openCancelFormButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                initCancellationForm();
            }
        });
    }


    private void initCancellationForm() {
        getContentPane().removeAll();
        repaint();
        setTitle("Cancellation Form");
        setSize(300, 150);

        JPanel panel = new JPanel();
        panel.setLayout(null);

        JLabel pnrLabel = new JLabel("PNR Number:");
        pnrLabel.setBounds(10, 10, 100, 25);
        panel.add(pnrLabel);

        pnrField = new JTextField(20);
        pnrField.setBounds(120, 10, 150, 25);
        panel.add(pnrField);

        cancelButton = new JButton("Cancel");
        cancelButton.setBounds(120, 50, 80, 25);
        panel.add(cancelButton);

        add(panel);
        setVisible(true);

        cancelButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String pnr = pnrField.getText();
                boolean found = false;

                for (Reservation reservation : reservations) {
                    if (reservation.getPnr().equals(pnr)) {
                        reservations.remove(reservation);
                        JOptionPane.showMessageDialog(null, "Reservation cancelled successfully!");
                        found = true;
                        break;
                    }
                }

                if (!found) {
                    JOptionPane.showMessageDialog(null, "PNR not found!");
                }
            }
        });
    }


    class Reservation {
        private String pnr;
        private String name;
        private String trainNumber;
        private String trainName;
        private String classType;
        private String date;
        private String from;
        private String to;

        public Reservation(String name, String trainNumber, String trainName, String classType, String date, String from, String to) {
            this.pnr = UUID.randomUUID().toString();
            this.name = name;
            this.trainNumber = trainNumber;
            this.trainName = trainName;
            this.classType = classType;
            this.date = date;
            this.from = from;
            this.to = to;
        }

        public String getPnr() {
            return pnr;
        }


        @Override
        public String toString() {
            return "PNR: " + pnr + ", Name: " + name + ", Train Number: " + trainNumber + ", Train Name: " + trainName + ", Class Type: " + classType + ", Date: " + date + ", From: " + from + ", To: " + to;
        }
    }

    // Main method to start the application
    public static void main(String[] args) {
        new OnlineReservationSystem();
    }
}




//TASK-2   NUMBER GUESSING GAME

import javax.swing.*;
import java.util.Random;

public class GuessTheNumber {

    public static void main(String[] args) {
        int maxAttempts = 10; // Maximum number of attempts
        int range = 100; // Range of the number to be guessed (1 to 100)
        int rounds = 3; // Number of rounds
        int totalScore = 0; // Total score

        for (int round = 1; round <= rounds; round++) {
            int attempts = 0;
            int score = 0;
            Random random = new Random();
            int numberToGuess = random.nextInt(range) + 1;

            JOptionPane.showMessageDialog(null, "Round " + round + " of " + rounds + "\nGuess a number between 1 and " + range);

            while (attempts < maxAttempts) {
                String input = JOptionPane.showInputDialog("Enter your guess (Attempt " + (attempts + 1) + " of " + maxAttempts + "):");
                if (input == null) {
                    JOptionPane.showMessageDialog(null, "Game cancelled");
                    return; // Exit if the user cancels the input
                }

                int guess;
                try {
                    guess = Integer.parseInt(input);
                } catch (NumberFormatException e) {
                    JOptionPane.showMessageDialog(null, "Invalid input. Please enter a number.");
                    continue;
                }

                attempts++;

                if (guess < 1 || guess > range) {
                    JOptionPane.showMessageDialog(null, "Please enter a number between 1 and " + range);
                } else if (guess < numberToGuess) {
                    JOptionPane.showMessageDialog(null, "Higher!");
                } else if (guess > numberToGuess) {
                    JOptionPane.showMessageDialog(null, "Lower!");
                } else {
                    score = (maxAttempts - attempts + 1) * 10; // Calculate score based on attempts
                    totalScore += score;
                    JOptionPane.showMessageDialog(null, "Correct! You've guessed the number in " + attempts + " attempts. You scored " + score + " points.");
                    break;
                }
            }

            if (attempts == maxAttempts) {
                JOptionPane.showMessageDialog(null, "Out of attempts! The number was " + numberToGuess);
            }
        }

        JOptionPane.showMessageDialog(null, "Game Over! Your total score is " + totalScore + " points.");
    }
}



//TASK-3 ATM INTERFACE 

import java.util.HashMap;
import java.util.Scanner;

public class ATMSystem {
    private static HashMap<String, String> users = new HashMap<>();
    private static HashMap<String, Double> balances = new HashMap<>();
    private static Scanner scanner = new Scanner(System.in);
    private static String currentUser;

    public static void main(String[] args) {

        users.put("user1", "1234");
        users.put("user2", "5678");
        balances.put("user1", 1000.0);
        balances.put("user2", 2000.0);


        System.out.println("Welcome to the ATM System");
        login();


        while (true) {
            System.out.println("\nSelect an option:");
            System.out.println("1. Check Balance");
            System.out.println("2. Withdraw");
            System.out.println("3. Deposit");
            System.out.println("4. Transfer");
            System.out.println("5. Quit");

            int choice = scanner.nextInt();
            switch (choice) {
                case 1:
                    checkBalance();
                    break;
                case 2:
                    withdraw();
                    break;
                case 3:
                    deposit();
                    break;
                case 4:
                    transfer();
                    break;
                case 5:
                    System.out.println("Thank you for using the ATM System. Goodbye!");
                    System.exit(0);
                default:
                    System.out.println("Invalid option! Please select a valid option.");
            }
        }
    }


    private static void login() {
        System.out.print("Enter your user ID: ");
        String userId = scanner.next();
        System.out.print("Enter your PIN: ");
        String pin = scanner.next();

        if (users.containsKey(userId) && users.get(userId).equals(pin)) {
            currentUser = userId;
            System.out.println("Login successful! Welcome, " + userId + "!");
        } else {
            System.out.println("Invalid credentials! Please try again.");
            login();
        }
    }


    private static void checkBalance() {
        System.out.println("Your current balance is: $" + balances.get(currentUser));
    }


    private static void withdraw() {
        System.out.print("Enter the amount to withdraw: $");
        double amount = scanner.nextDouble();
        double balance = balances.get(currentUser);

        if (amount > balance) {
            System.out.println("Insufficient funds! Your current balance is: $" + balance);
        } else {
            balances.put(currentUser, balance - amount);
            System.out.println("Withdrawal successful! Remaining balance: $" + (balance - amount));
        }
    }


    private static void deposit() {
        System.out.print("Enter the amount to deposit: $");
        double amount = scanner.nextDouble();
        double balance = balances.get(currentUser);
        balances.put(currentUser, balance + amount);
        System.out.println("Deposit successful! New balance: $" + (balance + amount));
    }


    private static void transfer() {
        System.out.print("Enter the recipient's user ID: ");
        String recipient = scanner.next();
        if (!users.containsKey(recipient)) {
            System.out.println("Recipient not found!");
            return;
        }

        System.out.print("Enter the amount to transfer: $");
        double amount = scanner.nextDouble();
        double senderBalance = balances.get(currentUser);
        double recipientBalance = balances.get(recipient);

        if (amount > senderBalance) {
            System.out.println("Insufficient funds! Your current balance is: $" + senderBalance);
        } else {
            balances.put(currentUser, senderBalance - amount);
            balances.put(recipient, recipientBalance + amount);
            System.out.println("Transfer successful! Your remaining balance: $" + (senderBalance - amount));
        }
    }
}


//TASK-4 ONLINE EXAMINATION

import java.util.Scanner;

public class OnlineExaminationSystem {
    private static String username;
    private static String password;
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        login();
        updateProfileAndPassword();
        selectAnswersForMCQs();
        timerWithAutoSubmit();
        closeSessionAndLogout();
    }

    private static void login() {
        System.out.println("Welcome to the Online Examination System");
        System.out.print("Enter username: ");
        username = scanner.nextLine();
        System.out.print("Enter password: ");
        password = scanner.nextLine();
        System.out.println("Login successful!");
    }

    private static void updateProfileAndPassword() {
        System.out.println("Profile Update");
        // Code to update profile and password
    }

    private static void selectAnswersForMCQs() {
        System.out.println("MCQs");
        // Code to select answers for MCQs
    }

    private static void timerWithAutoSubmit() {
        System.out.println("Timer started...");
        // Code for timer with auto-submit
    }

    private static void closeSessionAndLogout() {
        System.out.println("Session closed. Logout successful.");
    }
}


//TASK-5 DIGITAL LIBRARY MANAGEMENT

import java.util.HashMap;
import java.util.Scanner;

public class DigitalLibraryManagement {
    private static HashMap<String, Book> books = new HashMap<>();
    private static HashMap<String, User> users = new HashMap<>();
    private static Scanner scanner = new Scanner(System.in);
    private static String currentUser;

    public static void main(String[] args) {

        users.put("admin", new User("admin", "admin123", true));
        users.put("user1", new User("user1", "password1", false));


        login();
    }


    private static void login() {
        System.out.println("Welcome to the Digital Library Management System");
        System.out.print("Enter your username: ");
        String username = scanner.nextLine().trim();
        System.out.print("Enter your password: ");
        String password = scanner.nextLine().trim();

        if (users.containsKey(username) && users.get(username).password.equals(password)) {
            currentUser = username;
            User user = users.get(username);

            if (user.isAdmin) {
                adminMenu();
            } else {
                userMenu();
            }
        } else {
            System.out.println("Invalid credentials! Please try again.");
            login();
        }
    }

    // Admin menu
    private static void adminMenu() {
        while (true) {
            System.out.println("\nAdmin Menu:");
            System.out.println("1. Add Book");
            System.out.println("2. Update Book");
            System.out.println("3. Delete Book");
            System.out.println("4. Logout");

            int choice = Integer.parseInt(scanner.nextLine());
            switch (choice) {
                case 1:
                    addBook();
                    break;
                case 2:
                    updateBook();
                    break;
                case 3:
                    deleteBook();
                    break;
                case 4:
                    System.out.println("Logging out...");
                    login();
                    return;
                default:
                    System.out.println("Invalid option! Please select a valid option.");
            }
        }
    }

    // User menu
    private static void userMenu() {
        while (true) {
            System.out.println("\nUser Menu:");
            System.out.println("1. View Books");
            System.out.println("2. Search Book");
            System.out.println("3. Issue Book");
            System.out.println("4. Return Book");
            System.out.println("5. Logout");

            int choice = Integer.parseInt(scanner.nextLine());
            switch (choice) {
                case 1:
                    viewBooks();
                    break;
                case 2:
                    searchBook();
                    break;
                case 3:
                    issueBook();
                    break;
                case 4:
                    returnBook();
                    break;
                case 5:
                    System.out.println("Logging out...");
                    login();
                    return;
                default:
                    System.out.println("Invalid option! Please select a valid option.");
            }
        }
    }

    // Add book function
    private static void addBook() {
        System.out.print("Enter book ID: ");
        String bookID = scanner.nextLine().trim();
        System.out.print("Enter book name: ");
        String bookName = scanner.nextLine().trim();
        System.out.print("Enter author name: ");
        String authorName = scanner.nextLine().trim();

        Book book = new Book(bookID, bookName, authorName);
        books.put(bookID, book);
        System.out.println("Book added successfully!");
    }

    // Update book function
    private static void updateBook() {
        System.out.print("Enter book ID to update: ");
        String bookID = scanner.nextLine().trim();

        if (books.containsKey(bookID)) {
            System.out.print("Enter new book name: ");
            String bookName = scanner.nextLine().trim();
            System.out.print("Enter new author name: ");
            String authorName = scanner.nextLine().trim();

            Book book = books.get(bookID);
            book.name = bookName;
            book.author = authorName;
            System.out.println("Book updated successfully!");
        } else {
            System.out.println("Book not found!");
        }
    }

    // Delete book function
    private static void deleteBook() {
        System.out.print("Enter book ID to delete: ");
        String bookID = scanner.nextLine().trim();

        if (books.containsKey(bookID)) {
            books.remove(bookID);
            System.out.println("Book deleted successfully!");
        } else {
            System.out.println("Book not found!");
        }
    }

    // View books function
    private static void viewBooks() {
        if (books.isEmpty()) {
            System.out.println("No books available.");
        } else {
            for (Book book : books.values()) {
                System.out.println(book);
            }
        }
    }

    // Search book function
    private static void searchBook() {
        System.out.print("Enter book name or author: ");
        String query = scanner.nextLine().trim().toLowerCase();

        boolean found = false;
        for (Book book : books.values()) {
            if (book.name.toLowerCase().contains(query) || book.author.toLowerCase().contains(query)) {
                System.out.println(book);
                found = true;
            }
        }

        if (!found) {
            System.out.println("No books found matching the query.");
        }
    }

    // Issue book function
    private static void issueBook() {
        System.out.print("Enter book ID to issue: ");
        String bookID = scanner.nextLine().trim();

        if (books.containsKey(bookID)) {
            Book book = books.get(bookID);
            if (book.isIssued) {
                System.out.println("Book is already issued.");
            } else {
                book.isIssued = true;
                System.out.println("Book issued successfully!");
            }
        } else {
            System.out.println("Book not found!");
        }
    }

    // Return book function
    private static void returnBook() {
        System.out.print("Enter book ID to return: ");
        String bookID = scanner.nextLine().trim();

        if (books.containsKey(bookID)) {
            Book book = books.get(bookID);
            if (!book.isIssued) {
                System.out.println("Book was not issued.");
            } else {
                book.isIssued = false;
                System.out.println("Book returned successfully!");
            }
        } else {
            System.out.println("Book not found!");
        }
    }

    // Book class
    static class Book {
        String id;
        String name;
        String author;
        boolean isIssued;

        Book(String id, String name, String author) {
            this.id = id;
            this.name = name;
            this.author = author;
            this.isIssued = false;
        }

        @Override
        public String toString() {
            return "Book ID: " + id + ", Name: " + name + ", Author: " + author + ", Issued: " + (isIssued ? "Yes" : "No");
        }
    }

    // User class
    static class User {
        String username;
        String password;
        boolean isAdmin;

        User(String username, String password, boolean isAdmin) {
            this.username = username;
            this.password = password;
            this.isAdmin = isAdmin;
        }
    }
}
