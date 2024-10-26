Task 1: Start Apache and mysql from xampp.




USE campaign_feedback;

CREATE TABLE feedback (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(50) NOT NULL,
    feedback TEXT NOT NULL,
    rating TINYINT,
    submission_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Feedback Form</title>
    <style>
        /* Updated form styling */
        body { font-family: Verdana, sans-serif; background-color: #eef2f7; }
        .form-container { max-width: 450px; margin: 20px auto; padding: 30px; background: #ffffff; border-radius: 10px; box-shadow: 0 0 15px rgba(0,0,0,0.1); }
        label { font-weight: bold; margin-top: 15px; display: block; color: #333; }
        input, textarea { width: 100%; padding: 12px; margin-top: 5px; font-size: 15px; border: 1px solid #ccc; border-radius: 4px; }
        button { background-color: #4285f4; color: #ffffff; font-weight: bold; padding: 12px; margin-top: 20px; width: 100%; border: none; cursor: pointer; border-radius: 4px; }
    </style>
    <script>
        function validateFeedback() {
            const name = document.forms["feedback"]["name"].value;
            const email = document.forms["feedback"]["email"].value;
            const rating = document.forms["feedback"]["rating"].value;
            if (!name || !email || !rating) {
                alert("Please fill out all required fields.");
                return false;
            }
            return true;
        }
    </script>
</head>
<body>
    <div class="form-container">
        <h2>We Value Your Feedback</h2>
        <form name="feedback" action="process_feedback.php" method="POST" onsubmit="return validateFeedback()">
            <label>Name:</label>
            <input type="text" name="name" required>
            
            <label>Email:</label>
            <input type="email" name="email" required>
            
            <label>Feedback:</label>
            <textarea name="feedback" required></textarea>
            
            <label>Rating (1-5):</label>
            <input type="number" name="rating" min="1" max="5" required>
            
            <button type="submit">Submit</button>
        </form>
    </div>
</body>
</html>




<?php
// Database connection
$host = "localhost";
$user = "root";
$pass = "";
$dbname = "campaign_feedback";

$conn = new mysqli($host, $user, $pass, $dbname);

if ($conn->connect_error) {
    die("Failed to connect: " . $conn->connect_error);
}

$name = filter_input(INPUT_POST, 'name', FILTER_SANITIZE_STRING);
$email = filter_input(INPUT_POST, 'email', FILTER_SANITIZE_EMAIL);
$feedback = filter_input(INPUT_POST, 'feedback', FILTER_SANITIZE_STRING);
$rating = filter_input(INPUT_POST, 'rating', FILTER_VALIDATE_INT);

$query = "INSERT INTO feedback (name, email, feedback, rating) VALUES (?, ?, ?, ?)";
$stmt = $conn->prepare($query);
$stmt->bind_param("sssi", $name, $email, $feedback, $rating);

if ($stmt->execute()) {
    echo "Thank you for your feedback!";
} else {
    echo "Submission failed. Please try again.";
}

$stmt->close();
$conn->close();
?>




<?php
// Database connection
$host = "localhost";
$user = "root";
$pass = "";
$dbname = "campaign_feedback";

$conn = new mysqli($host, $user, $pass, $dbname);

// Check for connection issues
if ($conn->connect_error) {
    die("Database connection error: " . $conn->connect_error);
}

// Retrieve feedback records
$query = "SELECT * FROM feedback ORDER BY submission_date DESC";
$result = $conn->query($query);
?>




<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Feedback Received</title>
    <style>
        /* Updated styling for feedback display */
        body { font-family: 'Segoe UI', Tahoma, sans-serif; background-color: #f0f7ff; }
        .table-container { max-width: 800px; margin: 25px auto; background: #fff; padding: 25px; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.2); }
        table { width: 100%; border-collapse: collapse; }
        th, td { padding: 14px; border-bottom: 1px solid #ddd; text-align: left; }
        th { background-color: #333; color: #ffffff; font-weight: 600; }
    </style>
</head>
<body>
    <div class="table-container">
        <h2>Feedback Entries</h2>
        <table>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Email</th>
                <th>Feedback</th>
                <th>Rating</th>
                <th>Submission Date</th>
            </tr>
            <?php if ($result->num_rows > 0): ?>
                <?php while($row = $result->fetch_assoc()): ?>
                    <tr>
                        <td><?= htmlspecialchars($row['id']) ?></td>
                        <td><?= htmlspecialchars($row['name']) ?></td>
                        <td><?= htmlspecialchars($row['email']) ?></td>
                        <td><?= htmlspecialchars($row['feedback']) ?></td>
                        <td><?= htmlspecialchars($row['rating']) ?></td>
                        <td><?= htmlspecialchars($row['submission_date']) ?></td>
                    </tr>
                <?php endwhile; ?>
            <?php else: ?>
                <tr><td colspan="6">No feedback found.</td></tr>
            <?php endif; ?>
        </table>
    </div>
</body>
</html>

<?php
$conn->close();
?>



Testing and validation:
Submit Form: Access feedback_form.html, enter sample data, and submit to confirm entries are saved.
Verify Display: Open display_feedback.php to ensure stored feedback displays accurately.
Cross-Browser Check: Test in Chrome, Firefox, Edge, and Safari to confirm consistent styling and functionality.







