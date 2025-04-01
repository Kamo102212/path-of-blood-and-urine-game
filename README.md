# path-of-blood-and-urine-game
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Path of Blood and Urine Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f1f1f1;
      text-align: center;
    }
    h1 {
      color: #4CAF50;
    }
    .card-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin-top: 20px;
    }
    .card {
      width: 200px;
      height: 50px;
      margin: 5px 0;
      background-color: #ffffff;
      border: 2px solid #000;
      font-size: 18px;
      text-align: center;
      vertical-align: middle;
      line-height: 50px;
      cursor: pointer;
      user-select: none;
    }
    .card.dragging {
      opacity: 0.5;
    }
    #result {
      margin-top: 20px;
      font-size: 18px;
    }
    button {
      padding: 10px 20px;
      margin-top: 20px;
      background-color: #4CAF50;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }
    button:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>

  <h1>Path of Blood and Urine</h1>
  <p>Arrange the cards in the correct order!</p>
  
  <div class="card-container" id="cardContainer">
    <!-- Cards will be dynamically inserted here -->
  </div>
  
  <button id="checkButton">Check if Correct</button>
  <div id="result"></div>
  
  <script>
    const correctOrder = [
      "Renal Artery", "Interlobular Veins", "Renal Pyramid", "Nephron", 
      "Glomerulus", "Capillaries", "Renal Vein", "Inferior Vena Cava",
      "Bowman's Capsule", "Proximal Tubule", "Loop of Henle", "Distal Tubule",
      "Collecting Duct", "Renal Pelvis", "Ureter", "Bladder", "Urethra = PEE!!!"
    ];

    let shuffledOrder = [...correctOrder];
    shuffledOrder.sort(() => Math.random() - 0.5); // Shuffle the cards

    const cardContainer = document.getElementById('cardContainer');
    const checkButton = document.getElementById('checkButton');
    const resultDiv = document.getElementById('result');

    // Create cards dynamically based on the shuffled order
    shuffledOrder.forEach((item, index) => {
      const card = document.createElement('div');
      card.classList.add('card');
      card.setAttribute('draggable', true);
      card.textContent = item;
      card.setAttribute('id', 'card' + index);
      card.addEventListener('dragstart', dragStart);
      card.addEventListener('dragover', dragOver);
      card.addEventListener('drop', dropCard);
      card.addEventListener('dragenter', dragEnter);
      card.addEventListener('dragleave', dragLeave);
      cardContainer.appendChild(card);
    });

    // Dragging card functionality
    let draggedCard = null;

    function dragStart(e) {
      draggedCard = e.target;
      e.dataTransfer.effectAllowed = 'move';
      e.dataTransfer.setData('text/html', draggedCard.innerHTML);
      draggedCard.classList.add('dragging');
    }

    function dragOver(e) {
      e.preventDefault();
      e.dataTransfer.dropEffect = 'move';
    }

    function dropCard(e) {
      e.preventDefault();
      const droppedCard = e.target;

      if (droppedCard !== draggedCard && droppedCard.classList.contains('card')) {
        const draggedIndex = [...cardContainer.children].indexOf(draggedCard);
        const droppedIndex = [...cardContainer.children].indexOf(droppedCard);

        // Swap cards in the container
        if (draggedIndex < droppedIndex) {
          cardContainer.insertBefore(droppedCard, draggedCard);
          cardContainer.insertBefore(draggedCard, cardContainer.children[droppedIndex]);
        } else {
          cardContainer.insertBefore(draggedCard, droppedCard);
          cardContainer.insertBefore(droppedCard, cardContainer.children[draggedIndex]);
        }

        updateCardOrder();
      }

      draggedCard.classList.remove('dragging');
      draggedCard = null;
    }

    function dragEnter(e) {
      e.target.style.borderColor = '#45a049'; // Highlight the card while dragging over it
    }

    function dragLeave(e) {
      e.target.style.borderColor = '#000'; // Reset the border color
    }

    // Function to update the order of cards after dragging
    function updateCardOrder() {
      const updatedOrder = [...cardContainer.children].map(card => card.textContent);
      shuffledOrder = updatedOrder;
    }

    // Check if the order is correct
    checkButton.addEventListener('click', () => {
      if (JSON.stringify(shuffledOrder) === JSON.stringify(correctOrder)) {
        resultDiv.textContent = "Congratulations! You've got the correct path!";
        resultDiv.style.color = 'green';
      } else {
        resultDiv.textContent = "Oops! The order is not correct. Try again!";
        resultDiv.style.color = 'red';
      }
    });
  </script>
</body>
</html>
   


  
