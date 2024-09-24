<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grade Calculator</title>
    <link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css">
    <script defer src="https://pyscript.net/latest/pyscript.js"></script>
    
    <style>
        body {
            font-family:'Franklin Gothic Medium', sans-serif;
            background-color: #f0f8ff;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        h1 {
            color: #333;
        }
        label, input, button {
            margin: 10px 0;
        }
        button {
            padding: 10px 20px;
            background-color: #e4507c;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #000ba8;
        }
        #results {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            background-color: #fff;
            width: 100%;
            max-width: 400px;
            text-align: center;
        }
    </style>
</head>
<body>
    <!-- Page title -->
    <h1>Grade Calculator</h1>
    <!-- Input field for Prelim grade -->
    <label for="prelim">Enter Prelim Grade (0-100):</label>
    <input type="number" id="prelim" min="0" max="100"><br><br>
    <!-- Button to trigger grade calculation -->
    <button id="calculate-btn">Calculate</button>
    
    <!-- Section to display results -->
    <h2>Results</h2>
    <p id="results"></p>

    <!-- PyScript code for grade calculation -->
    <py-script>
        from pyscript import Element

        def calculate_grades(*args):
            # Get the value from the Prelim grade input field
            prelim = Element("prelim").element.value
            results = Element("results")
            
            try:
                # Convert the input to a float and validate the range
                prelim = float(prelim)
                if prelim < 0 or prelim > 100:
                    results.element.innerHTML = "Please input a valid numerical grade for your Prelim, ranging from 0 to 100."
                    return
                
                passing_grade = 75
                dean_lister_grade = 90
                
                # Calculate required Midterm and Final grades to pass
                required_midterm = (passing_grade - (prelim * 0.2)) / 0.8
                required_final = (passing_grade - (prelim * 0.2)) / 0.5
                
                # Cap the required Final grade at 100
                if required_final > 100:
                    required_final = 100
                
                # Calculate required Midterm and Final grades to be a Dean's Lister
                required_midterm_dean = (dean_lister_grade - (prelim * 0.2)) / 0.8
                required_final_dean = (dean_lister_grade - (prelim * 0.2)) / 0.5
                
                # Ensure Dean's Lister grades are between 90 and 100
                if required_midterm_dean < 90:
                    required_midterm_dean = 90
                if required_midterm_dean > 100:
                    required_midterm_dean = 100
                if required_final_dean < 90:
                    required_final_dean = 90
                if required_final_dean > 100:
                    required_final_dean = 100
                
                # Determine if the student has a chance to pass
                if prelim >= passing_grade:
                    message = "You have already passed based on your Prelim grade."
                elif required_midterm <= 100:
                    message = f"To pass, you need at least {required_midterm:.2f} in Midterm and {required_final:.2f} in Final."
                else:
                    message = "Unfortunately, your current Prelim grade won't be enough to pass, but there's still time to improve."
                
                # Check for Dean's Lister qualification
                if prelim >= dean_lister_grade:
                    message += "<br>Keep up the great work! You're on track to become a Dean's Lister. Just maintain an average of 90 or higher."
                
                # Display the required grades
                message += f"<br>Prelim Grade: {prelim:.2f}"
                message += f"<br>Minimum Midterm Grade: {required_midterm:.2f}"
                message += f"<br>Dean's Lister Midterm Grade: {required_midterm_dean:.2f}"
                message += f"<br>Maximum Final Grade: {required_final:.2f}"
                message += f"<br>Dean's Lister Final Grade: {required_final_dean:.2f}"
                
                results.element.innerHTML = message
            
            except ValueError:
                results.element.innerHTML = "Please input a valid numerical grade for your Prelim, ranging from 0 to 100."

        # Bind the calculate_grades function to the button click event
        Element("calculate-btn").element.onclick = calculate_grades
    </py-script>
</body>
</html>
