Certainly! Let's go through the code line by line:

javascript
Copy code
// Language dictionary
const languages = {
  "am-ET": "Amharic",
  "ar-SA": "Arabic",
  // ... (rest of the languages)
  "yi-YD": "Yiddish",
  "zu-ZA": "Zulu"
};
This section defines a constant languages that stores a dictionary of language codes as keys and corresponding language names as values.

javascript
Copy code
// DOM elements
const fromText = document.querySelector(".from-text");
const toText = document.querySelector(".to-text");
const exchangeIcon = document.querySelector(".exchange");
const selectTags = document.querySelectorAll("select");
const icons = document.querySelectorAll(".row ion-icon");
const translateBtn = document.querySelector("button");
Here, we declare constants to store references to various HTML elements using document.querySelector and document.querySelectorAll.

javascript
Copy code
// Populate language options in select tags
selectTags.forEach((tag, id) => {
  for (let langCode in languages) {
    let selected = (id === 0 && langCode === "en-GB") || (id === 1 && langCode === "fa-IR") ? "selected" : "";
    let option = `<option ${selected} value="${langCode}">${languages[langCode]}</option>`;
    tag.insertAdjacentHTML("beforeend", option);
  }
});
This section populates the options in the select tags for choosing the source and target languages. It uses a nested loop to iterate over the language dictionary and create HTML options, setting the "selected" attribute for default languages.

javascript
Copy code
// Event listener for input
fromText.addEventListener("keyup", () => {
  if (!fromText.value) {
    toText.value = "";
  }
});
This event listener triggers when there is a keyup event in the "fromText" input field. If the input is empty, it clears the "toText" field.

javascript
Copy code
// Event listener for translate button
translateBtn.addEventListener("click", () => {
  let text = fromText.value.trim();
  let translateFrom = selectTags[0].value;
  let translateTo = selectTags[1].value;

  if (!text) return;

  toText.setAttribute("placeholder", "Translating...");

  let apiUrl = `https://api.mymemory.translated.net/get?q=${text}&langpair=${translateFrom}|${translateTo}`;

  fetch(apiUrl)
    .then((res) => res.json())
    .then((data) => {
      toText.value = data.responseData.translatedText;
      data.matches.forEach((data) => {
        if (data.id === 0) {
          toText.value = data.translation;
        }
      });
      toText.setAttribute("placeholder", "Translation");
    });
});
This event listener is triggered when the translate button is clicked. It retrieves the input text, source language, and target language. It then constructs a URL for translation using the MyMemory API and fetches the translation data. The result is then displayed in the "toText" field.

javascript
Copy code
// Event listener for exchange icon
exchangeIcon.addEventListener("click", () => {
  let tempText = fromText.value;
  let tempLang = selectTags[0].value;

  fromText.value = toText.value;
  toText.value = tempText;

  selectTags[0].value = selectTags[1].value;
  selectTags[1].value = tempLang;
});
This event listener is triggered when the exchange icon is clicked. It swaps the content of the "fromText" and "toText" fields as well as the selected languages in the source and target language select tags.

javascript
Copy code
// Event listeners for icons
icons.forEach((icon) => {
  icon.addEventListener("click", ({ target }) => {
    if (!fromText.value || !toText.value) return;

    if (target.getAttribute("name") === "copy-outline") {
      if (target.id === "from") {
        navigator.clipboard.writeText(fromText.value);
      } else {
        navigator.clipboard.writeText(toText.value);
      }
    } else {
      let utterance;
      if (target.id === "from") {
        utterance = new SpeechSynthesisUtterance(fromText.value);
        utterance.lang = selectTags[0].value;
      } else {
        utterance = new SpeechSynthesisUtterance(toText.value);
        utterance.lang = selectTags[1].value;
      }
      speechSynthesis.speak(utterance);
    }
  });
});
This section adds event listeners to the icons for copy and speak functionality. If the copy icon is clicked, it uses the Clipboard API to copy the content of the corresponding text field. If the speak icon is clicked, it uses the Speech Synthesis API to read the content of the corresponding text field aloud.





