package profile;
import java.io.*;
import java.util.ArrayList;
import java.util.Scanner;

public class SocialMediaApp {
    private static final String USERS_FILE = "users1.txt";
    private static final String FRIEND_REQUESTS_FILE = "friend_requests.txt";
    private static final String FRIENDS_FILE = "friends.txt";
    private static final String POSTS_FILE = "posts.txt";
    private static final String COMMENTS_FILE = "comments.txt";
    private static final String NOTIFICATIONS_FILE = "notifications.txt";

    public static void main(String[] args) throws IOException {
        createFile("users1.txt");
        createFile("friend_requests.txt");
        createFile("friends.txt");
        createFile("posts.txt");
        createFile("comments.txt");
        createFile("notifications.txt");
        Scanner scanner = new Scanner(System.in);
        int choice = 0;
        while (choice != 6) {
            System.out.println("Social Media Application");
            System.out.println("1. Create Profile");
            System.out.println("2. Connect with Friends");
            System.out.println("3. Share Post");
            System.out.println("4. Comment on Post");
            System.out.println("5. View Notifications");
            System.out.println("6. Quit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // consume the newline character

            switch (choice) {
                case 1:
                    createProfile(scanner);
                    break;
                case 2:
                    connectWithFriends(scanner);
                    break;
                case 3:
                    sharePost(scanner);
                    break;
                case 4:
                    commentOnPost(scanner);
                    break;
                case 5:
                    viewNotifications(scanner);
                    break;
                case 6:
                    System.out.println("Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice.");
                    break;
            }
        }
    }

    private static void createProfile(Scanner scanner) {
        System.out.print("Enter your username: ");
        String username = scanner.nextLine();
        System.out.print("Enter your password: ");
        String password = scanner.nextLine();

        // check if the user already has a profile
        ArrayList<String> users = null;
        try {
            users = readFromFile(USERS_FILE);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        for (String user : users) {
            String[] tokens = user.split(",");
            if (tokens[0].equals(username)) {
                System.out.println("A profile already exists for this username.");
                return;
            }
        }

        // create the user's profile
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();
        System.out.print("Enter your profile picture URL: ");
        String profilePictureUrl = scanner.nextLine();
        System.out.print("Enter your bio: ");
        String bio = scanner.nextLine();

        // save the user's information to file
        try {
            FileWriter writer = new FileWriter(new File(USERS_FILE), true);
            writer.write(username + "," + password + "," + name + "," + profilePictureUrl + "," + bio + "\n");
            writer.close();
            System.out.println("Profile created successfully.");
        } catch (IOException e) {
            System.out.println("Failed to create profile.");
            e.printStackTrace();
        }
    }
    private static void connectWithFriends(Scanner scanner) throws IOException {
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();
        // display the user's pending friend requests
        ArrayList<String> friendRequests = readFromFile(FRIEND_REQUESTS_FILE);
        System.out.println("Pending friend requests:");
        for (String friendRequest : friendRequests) {
            String[] tokens = friendRequest.split(",");
            if (tokens[1].equals(name)) {
                System.out.println(tokens[0]);
            }
        }

        // display the user's friends
        ArrayList<String> friends = readFromFile(FRIENDS_FILE);
        System.out.println("Your friends:");
        for (String friend : friends) {
            String[] tokens = friend.split(",");
            if (tokens[0].equals(name)) {
                System.out.println(tokens[1]);
            } else if (tokens[1].equals(name)) {
                System.out.println(tokens[0]);
            }
        }

        // allow the user to send a friend request or accept/reject a pending friend request
        System.out.print("Enter 1 to send a friend request, 2 to accept a pending request, 3 to reject a pending request: ");
        int choice = scanner.nextInt();
        scanner.nextLine(); // consume the newline character

        switch (choice) {
            case 1:
                sendFriendRequest(scanner, name);
                break;
            case 2:
                acceptFriendRequest(scanner, name);
                break;
            case 3:
                rejectFriendRequest(scanner, name);
                break;
            default:
                System.out.println("Invalid choice.");
                break;
        }
    }

    private static void sendFriendRequest(Scanner scanner, String senderName) throws IOException {
        System.out.print("Enter the name of the person you want to send a friend request to: ");
        String recipientName = scanner.nextLine();

        // check if the recipient has already received a friend request from the sender
        ArrayList<String> friendRequests = null;
        try {
            friendRequests = readFromFile(FRIEND_REQUESTS_FILE);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        for (String friendRequest : friendRequests) {
            String[] tokens = friendRequest.split(",");
            if (tokens[0].equals(senderName) && tokens[1].equals(recipientName)) {
                System.out.println("You have already sent a friend request to " + recipientName + ".");
                return;
            } else if (tokens[0].equals(recipientName) && tokens[1].equals(senderName)) {
                System.out.println("You have already received a friend request from " + recipientName + ".");
                return;
            }
        }

        // add the friend request to file
        try {
            FileWriter writer = new FileWriter(new File(FRIEND_REQUESTS_FILE), true);
            writer.write(senderName + "," + recipientName + "\n");
            writer.close();
            System.out.println("Friend request sent successfully.");
        } catch (IOException e) {
            System.out.println("Failed to send friend request.");
            e.printStackTrace();
        }
    }
    public static ArrayList<String> readFromFile(String fileName) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader(fileName));
        ArrayList<String> lines = new ArrayList<String>();
        String line = null;
        while ((line = reader.readLine()) != null) {
            lines.add(line);
        }
        reader.close();
        return lines;
    }

    private static void acceptFriendRequest(Scanner scanner, String recipientName) throws IOException {
        System.out.print("Enter the name of the person whose friend request you want to accept: ");
        String senderName = scanner.nextLine();
// remove the friend request from the file
        ArrayList<String> friendRequests = null;
        try {
            friendRequests = readFromFile(FRIEND_REQUESTS_FILE);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        boolean foundRequest = false;
        for (int i = 0; i < friendRequests.size(); i++) {
            String friendRequest = friendRequests.get(i);
            String[] tokens = friendRequest.split(",");
            if (tokens[0].equals(senderName) && tokens[1].equals(recipientName)) {
                friendRequests.remove(i);
                foundRequest = true;
                break;
            }
        }
        if (!foundRequest) {
            System.out.println("You have not received a friend request from " + senderName + ".");
            return;
        }

// add the friends to the friends file
        try {
            FileWriter writer = new FileWriter(new File(FRIENDS_FILE), true);
            writer.write(senderName + "," + recipientName + "\n");
            writer.write(recipientName + "," + senderName + "\n");
            writer.close();
            System.out.println("You are now friends with " + senderName + "!");
        } catch (IOException e) {
            System.out.println("Failed to add friends.");
            e.printStackTrace();
        }
    }
    private static void rejectFriendRequest(Scanner scanner, String recipientName) throws IOException {
        System.out.print("Enter the name of the person whose friend request you want to reject: ");
        String senderName = scanner.nextLine();
// remove the friend request from the file
        ArrayList<String> friendRequests = readFromFile(FRIEND_REQUESTS_FILE);
        boolean foundRequest = false;
        for (int i = 0; i < friendRequests.size(); i++) {
            String friendRequest = friendRequests.get(i);
            String[] tokens = friendRequest.split(",");
            if (tokens[0].equals(senderName) && tokens[1].equals(recipientName)) {
                friendRequests.remove(i);
                foundRequest = true;
                break;
            }
        }
        if (!foundRequest) {
            System.out.println("You have not received a friend request from " + senderName + ".");
            return;
        }}
    private static void sharePost(Scanner scanner) {
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();
        System.out.print("Enter your post: ");
        String post = scanner.nextLine();

        // save the post to file
        try {
            FileWriter writer = new FileWriter(new File(POSTS_FILE), true);
            writer.write(name + "," + post + "\n");
            writer.close();
            System.out.println("Post shared successfully.");
        } catch (IOException e) {
            System.out.println("Failed to share post.");
            e.printStackTrace();
        }
    }

    private static void commentOnPost(Scanner scanner) {
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();
        System.out.print("Enter the post you want to comment on: ");
        String post = scanner.nextLine();
        System.out.print("Enter your comment: ");
        String comment = scanner.nextLine();

        // save the comment to file
        try {
            FileWriter writer = new FileWriter(new File(COMMENTS_FILE), true);
            writer.write(name + "," + post + "," + comment + "\n");
            writer.close();
            System.out.println("Comment posted successfully.");
        } catch (IOException e) {
            System.out.println("Failed to post comment.");
            e.printStackTrace();
        }
    }

    private static void viewNotifications(Scanner scanner) throws IOException {
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();

        // display the user's notifications
        ArrayList<String> notifications = readFromFile(NOTIFICATIONS_FILE);
        for (String notification : notifications) {
            String[] tokens = notification.split(",");
            if (tokens[1].equals(name)) {
                System.out.println(tokens[0]);
            }
        }
    }
    private static void createFile(String filename) {
        try {
            File file = new File(filename);
            file.createNewFile();
        } catch (IOException e) {
            System.out.println("An error occurred while creating the file: " + filename);
            e.printStackTrace();
        }
    }
}
