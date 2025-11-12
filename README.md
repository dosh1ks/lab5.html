<!DOCTYPE html>
<html lang="uk">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TechNova — Введення інформації про товари</title>
  <style>
    body {
      font-family: 'Gill Sans', 'Lucida Sans Unicode', sans-serif;
      background-color: #f4f8fb;
      color: #333;
      margin: 0;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: #003366;
    }

    form {
      max-width: 700px;
      margin: 25px auto;
      background: white;
      border-radius: 10px;
      padding: 25px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    fieldset {
      border: 2px solid #6699cc;
      border-radius: 8px;
      margin-bottom: 20px;
      padding: 15px;
    }

    legend {
      color: #336699;
      font-weight: bold;
      font-size: 1.1em;
    }

    label {
      display: block;
      margin-top: 10px;
      font-weight: 500;
    }

    input, textarea, select, meter {
      width: 100%;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 5px;
      margin-top: 5px;
    }

    input[type="submit"], button {
      background-color: #4c89e3;
      color: white;
      border: none;
      padding: 10px 15px;
      border-radius: 5px;
      margin-top: 15px;
      cursor: pointer;
      font-weight: bold;
    }

    input[type="submit"]:hover, button:hover {
      background-color: #3b72c0;
    }

    progress {
      width: 100%;
      height: 18px;
      margin-top: 10px;
    }

    .error {
      color: red;
      font-size: 14px;
      margin-top: 10px;
    }

    .success {
      color: green;
      font-weight: bold;
    }

    output {
      display: block;
      font-weight: bold;
      color: #004080;
      margin-top: 10px;
      background: #eef4fb;
      border-radius: 5px;
      padding: 10px;
    }
  </style>
</head>
<body>

  <h1>Форма введення інформації про товари компанії “TechNova”</h1>

  <form action="#" id="productForm" method="get">
    <fieldset>
      <legend>Основна інформація</legend>

      <label for="title">Назва товару:</label>
      <input type="text" id="title" name="title" required minlength="3" maxlength="40">

      <label for="category">Категорія:</label>
      <input list="categories" id="category" name="category" placeholder="Виберіть або введіть">
      <datalist id="categories">
        <option value="Смартфони">
        <option value="Ноутбуки">
        <option value="Навушники">
        <option value="Планшети">
      </datalist>

      <label for="price">Ціна (грн):</label>
      <input type="number" id="price" name="price" min="1" max="999999" required>

      <label for="rating">Рейтинг (1–10):</label>
      <input type="range" id="rating" name="rating" min="1" max="10" value="5">
      <meter id="meter" min="1" max="10" value="5" high="8" low="4"></meter>

      <label for="description">Опис товару:</label>
      <textarea id="description" name="description" rows="4" maxlength="300" required></textarea>
    </fieldset>

    <fieldset>
      <legend>Додаткова інформація</legend>

      <label for="release">Дата випуску:</label>
      <input type="date" id="release" name="release">

      <label for="available">Наявність:</label>
      <input type="checkbox" id="available" name="available"> Є в наявності

      <label for="progress">Заповнення форми:</label>
      <progress id="progress" value="0" max="100"></progress>
    </fieldset>

    <fieldset>
      <legend>Керування</legend>
      <button type="button" id="showSummary">Показати зведення</button>
      <input type="submit" value="Відправити інформацію">
    </fieldset>

    <output id="summary"></output>
    <p id="message" class="error"></p>
  </form>

  <script>
    const form = document.getElementById('productForm');
    const rating = document.getElementById('rating');
    const meter = document.getElementById('meter');
    const progress = document.getElementById('progress');
    const message = document.getElementById('message');
    const summary = document.getElementById('summary');
    const inputs = form.querySelectorAll('input, textarea');

    rating.addEventListener('input', () => {
      meter.value = rating.value;
    });

    // Підрахунок заповнення
    inputs.forEach(input => {
      input.addEventListener('input', () => {
        let filled = 0;
        inputs.forEach(i => { if (i.type !== "submit" && i.value.trim() !== '') filled++; });
        progress.value = Math.round((filled / (inputs.length - 2)) * 100);
      });
    });

    // Перевірка перед "відправкою"
    form.addEventListener('submit', e => {
      e.preventDefault();
      const name = document.getElementById('title').value.trim();
      const price = parseFloat(document.getElementById('price').value);

      if (name.length < 3) {
        message.textContent = "Назва товару має бути не коротшою за 3 символи!";
        message.className = "error";
      } else if (price <= 0 || isNaN(price)) {
        message.textContent = "Ціна має бути більшою за 0!";
        message.className = "error";
      } else {
        message.textContent = "Інформацію успішно введено!";
        message.className = "success";
        form.reset();
        progress.value = 0;
        meter.value = 5;
        summary.textContent = "";
      }
    });

    // Кнопка для показу зведення
    document.getElementById('showSummary').addEventListener('click', () => {
      const name = document.getElementById('title').value || "—";
      const category = document.getElementById('category').value || "—";
      const price = document.getElementById('price').value || "—";
      const ratingVal = document.getElementById('rating').value || "—";
      const description = document.getElementById('description').value || "—";
      const release = document.getElementById('release').value || "—";
      const available = document.getElementById('available').checked ? "Так" : "Ні";

      summary.innerHTML = `
        <strong>Назва:</strong> ${name}<br>
        <strong>Категорія:</strong> ${category}<br>
        <strong>Ціна:</strong> ${price} грн<br>
        <strong>Рейтинг:</strong> ${ratingVal}/10<br>
        <strong>Опис:</strong> ${description}<br>
        <strong>Дата випуску:</strong> ${release}<br>
        <strong>Наявність:</strong> ${available}
      `;
    });
  </script>

</body>
</html>
