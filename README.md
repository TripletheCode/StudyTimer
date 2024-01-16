<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weekly Meal Planner</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      text-align: center;
      margin: 20px;
      background-color: #f4f4f4; /* Light gray background */
      color: #000; /* Dark black text color */
    }
    h1 {
      color: #2e8b57; /* Sea Green header color */
    }
    #colorPicker {
      margin-top: 20px;
    }
    #calendar {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      grid-gap: 10px;
      margin-top: 20px;
      margin-bottom: 20px;
    }
    ul {
      list-style-type: none;
      padding: 0;
      margin: 0;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
    }
    li {
      margin: 10px;
      padding: 10px;
      background-color: #fff; /* White background for list items */
      border-radius: 5px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      transition: background-color 0.3s;
      color: #000; /* Dark black text color */
      position: relative;
    }
    li:hover {
      background-color: #f0f0f0; /* Light gray background on hover */
    }
    .remove-button {
      position: absolute;
      top: 15px;
      right: 15px;
      cursor: pointer;
      font-weight: bold;
      color: #ff0000; /* Red color for the remove button */
    }
    button {
      padding: 10px;
      margin: 10px;
      font-size: 16px;
      cursor: pointer;
      background-color: #2e8b57; /* Sea Green button color */
      color: white;
      border: none;
      border-radius: 5px;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: #228b22; /* Forest Green button color on hover */
    }
    form {
      margin-top: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    label {
      margin-bottom: 5px;
      color: #2e8b57; /* Sea Green label color */
    }
    input {
      margin-bottom: 10px;
      padding: 5px;
      border: 1px solid #ccc;
      border-radius: 3px;
    }
  </style>
</head>
<body>
  <h1>Weekly Meal Planner</h1>

  <!-- Color picker for background color selection -->
  <label for="colorPicker">Select background color:</label>
  <select id="colorPicker" onchange="changeBackgroundColor()">
    <option value="#a52a2a">Brown</option>
    <option value="#8b4513">Saddle Brown</option>
    <option value="#2f4f4f">Dark Slate Gray</option>
    <option value="#556b2f">Dark Olive Green</option>
    <option value="#483d8b">Dark Slate Blue</option>
    <option value="#006400">Dark Green</option>
    <option value="#8b0000">Dark Red</option>
    <option value="#8b008b">Dark Magenta</option>
    <option value="#87ceeb">Sky Blue</option>
    <option value="#f08080">Light Coral</option>
    <option value="#dda0dd">Plum</option>
    <option value="#7fffd4">Aquamarine</option>
  </select>

  <!-- Display calendar-style layout for the meal plan -->
  <div id="calendar"></div>

  <!-- Display shopping list -->
  <section id="shoppingList">
    <h2>Shopping List</h2>
    <ul id="listItems"></ul>
    <!-- Form to add and remove items from the shopping list -->
    <form id="listForm">
      <label for="itemName">Item Name:</label>
      <input type="text" id="itemName" required>
      <label for="itemQuantity">Quantity:</label>
      <input type="number" id="itemQuantity" required>
      <button type="button" onclick="addItemToList()">Add to List</button>
    </form>
  </section>

  <!-- Print button for shopping list -->
  <button onclick="printList('Shopping List', 'listItems')">Print Shopping List</button>

  <script>
    // Predefined list of meals with ingredients
    const predefinedMeals = {
      "Oatmeal": ["Oats", "Milk", "Fruits"],
      "Sandwich": ["Bread", "Cheese", "Tomato", "Lettuce", "Turkey"],
      "Grilled Chicken": ["Chicken Breast", "Olive Oil", "Herbs", "Vegetables"],
      "Pasta": ["Pasta", "Tomato Sauce", "Ground Beef", "Parmesan Cheese", "Roma Tomatoes"],
      "Salad": ["Lettuce", "Tomato", "Cucumber", "Dressing"],
      "Smoothie": ["Banana", "Yogurt", "Berries", "Honey"],
      "Stir Fry": ["Tofu", "Broccoli", "Soy Sauce", "Rice"],
      // ... (rest of the predefined meals)
    };

    // Function to generate a non-repeating meal plan and shopping list for the week
    function generateMealPlan() {
      // Get the days of the week
      const daysOfWeek = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

      // Initialize shopping list array
      const shoppingList = {};

      // Copy the predefined meals to avoid modification
      const availableMeals = { ...predefinedMeals };

      // Loop through each day and add a randomly selected meal without repeats
      daysOfWeek.forEach(day => {
        const availableMealNames = Object.keys(availableMeals);
        
        if (availableMealNames.length === 0) {
          // If all meals have been used, break out of the loop
          return;
        }

        const randomMealName = getRandomItem(availableMealNames);
        const ingredients = availableMeals[randomMealName];

        // Add the meal to the calendar
        const cell = document.createElement("div");
        cell.textContent = `${day}: ${randomMealName}`;
        cell.style.backgroundColor = "#87ceeb"; // Light blue background for days
        cell.style.padding = "10px";
        cell.style.borderRadius = "5px";
        document.getElementById("calendar").appendChild(cell);

        // Update the shopping list with quantities (number of times ingredient appears)
        ingredients.forEach(ingredient => {
          shoppingList[ingredient] = (shoppingList[ingredient] || 0) + 1;
        });

        // Remove the used meal from the available meals
        delete availableMeals[randomMealName];
      });

      // Display the shopping list
      displayShoppingList("listItems", shoppingList);
    }

    // Function to get a random item from an array
    function getRandomItem(array) {
      const randomIndex = Math.floor(Math.random() * array.length);
      return array[randomIndex];
    }

    // Function to display the shopping list with quantities
    function displayShoppingList(elementId, shoppingList) {
      const listElement = document.getElementById(elementId);
      listElement.innerHTML = "";

      Object.entries(shoppingList).forEach(([ingredient, quantity]) => {
        const listItem = document.createElement("li");
        listItem.textContent = `${ingredient} - Quantity: ${quantity}`;

        // Add a remove button at the top-right corner of each shopping list item
        const removeButton = document.createElement("span");
        removeButton.textContent = "x";
        removeButton.className = "remove-button";
        removeButton.onclick = function() { removeItemFromList(ingredient); };
        listItem.appendChild(removeButton);

        listElement.appendChild(listItem);
      });
    }

    // Function to remove the selected item from the shopping list
    function removeItemFromList(ingredient) {
      const listElement = document.getElementById("listItems");
      const items = listElement.getElementsByTagName("li");

      for (let i = 0; i < items.length; i++) {
        if (items[i].textContent.startsWith(ingredient)) {
          listElement.removeChild(items[i]);
          break;
        }
      }
    }

    // Function to change the background color
    function changeBackgroundColor() {
      const selectedColor = document.getElementById("colorPicker").value;
      document.body.style.backgroundColor = selectedColor;

      // Save the selected color to localStorage
      localStorage.setItem("backgroundColor", selectedColor);
    }

    // Function to print the specified list
    function printList(listName, elementId) {
      const listElement = document.getElementById(elementId);
      const listItems = listElement.getElementsByTagName("li");
      const printableList = Array.from(listItems).map(item => item.textContent).join('\n');

      // Create a new window for printing
      const printWindow = window.open('', '_blank');
      printWindow.document.write(`<pre>${listName}\n\n${printableList}</pre>`);
      printWindow.document.close();

      // Print the window
      printWindow.print();
    }

    // Function to add an additional item to the shopping list
    function addItemToList() {
      const itemName = document.getElementById("itemName").value;
      const itemQuantity = parseInt(document.getElementById("itemQuantity").value, 10);

      if (itemName && !isNaN(itemQuantity) && itemQuantity > 0) {
        const listElement = document.getElementById("listItems");
        const listItem = document.createElement("li");
        listItem.textContent = `${itemName} - Quantity: ${itemQuantity}`;

        // Add a remove button at the top-right corner of the new shopping list item
        const removeButton = document.createElement("span");
        removeButton.textContent = "x";
        removeButton.className = "remove-button";
        removeButton.onclick = function() { removeItemFromList(itemName); };
        listItem.appendChild(removeButton);

        listElement.appendChild(listItem);

        // Clear the form fields after adding the item
        document.getElementById("itemName").value = "";
        document.getElementById("itemQuantity").value = "";
      }
    }

    // Load the saved background color or set default when the page is loaded
    window.onload = function() {
      const savedColor = localStorage.getItem("backgroundColor");
      const defaultColor = document.getElementById("colorPicker").options[0].value;
      const backgroundColor = savedColor ? savedColor : defaultColor;

      document.body.style.backgroundColor = backgroundColor;
      document.getElementById("colorPicker").value = backgroundColor;

      // Delay the meal plan generation to allow smoother background color transition
      setTimeout(generateMealPlan, 100);
    };
  </script>
</body>
</html>


