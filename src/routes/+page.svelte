<script> 
import * as sanitizeHtml from 'sanitize-html';   

//API call
const API_URL = 'https://opentdb.com/api.php'  
    const fetchQuestion = async () => {
  loading = true;
  const url = new URL(API_URL)
  url.searchParams.append('amount', 1)
  url.searchParams.append('category', category)
  url.searchParams.append('difficulty', difficulty)
  url.searchParams.append('type', 'multiple')

  const response = await fetch(url)
  const data = await response.json()
  const questionData = data.results[0];
  question = questionData.question
  correctAnswer = questionData.correct_answer
  incorrectAnswers = questionData.incorrect_answers
  selectedAnswer = ''
  correct = null
  loading = false
}
//variables
    let question = ''
    let correctAnswer = ''
    let incorrectAnswers = []
    let selectedAnswer = ''
    let correct = null
    let loading = true
    let category = ''
    let difficulty = ''
    let correctCount = 0
    let wrongCount = 0
    let showSettings = false
    let clicked = false

    //functions
    function escapeString(string) {
  let escapedString = "";
  for (let i = 0; i < string.length; i++){
    if (string[i] === '"') {
      escapedString += '\\"';
    } else if (string[i] === "'") {
      escapedString += "\\'";
    } else if (string.slice(i, i + 6) === "&#039;") {
      escapedString += "'";
      i += 5;
    } else if (string.slice(i, i + 6) === "&quot;") {
      escapedString += '"';
      i += 5;
    } else {
      escapedString += string[i];
    }
  }
  return escapedString;
}

  function rightAnswer() {
    //maybe add more fuctionality, if I have time.
	correctCount++
	}

  function wrongAnswer() {
        //maybe add more fuctionality, if I have time.
	wrongCount++
	}
  
    const handleCategoryChange = (event) => {
      category = event.target.value
      fetchQuestion()
    }
  
    const handleDifficultyChange = (event) => {
      difficulty = event.target.value
      fetchQuestion()
    }
  
    const handleAnswerSelection = (event) => {
      if(clicked === false){
      selectedAnswer = event.target.value
      correct = selectedAnswer === correctAnswer
      if (correct) {
        rightAnswer()
      }
      else {
        wrongAnswer()
      }
      clicked = true
    }
else {
  return
}
    }
 
    const handleSubmit = (event) => {
      clicked = false
      event.preventDefault()
      fetchQuestion()
    }
    function toggleSettings() {
      showSettings = !showSettings
    }


  
    fetchQuestion()
  </script>
  
  
  <main> 

    {#if loading}
      <p>Loading question...</p>
    {:else}
    <div class="flex  justify-center ... pt-20">
      <h1 class="text-3xl font-bold">{escapeString(question)}</h1>
    </div>
      <form class="form-control" on:submit={handleSubmit}>
       
        <fieldset>
          <div class="flex  justify-center ... pt-20 text-2xl">
          <h2>Select an answer:</h2>
        </div> 
        <div class="flex justify-center ...">
          {#each [correctAnswer, ...incorrectAnswers].sort(() => Math.random() - 0.5) as answer}
          
          <label class="pt-6 pb-10 text-lg">
              <input type="radio" name="answer" class="radio radio-success ml-8" value={answer} on:change={handleAnswerSelection} />
               {sanitizeHtml(answer)}
             
            </label>
     
          {/each}
        </div>
      
        </fieldset>
        <div class="flex justify-center ...">
    
        <button class="btn btn-primary w-40" type="submit">Next question</button>
      </div>
      </form>
   
    {/if}

    <div class="flex justify-center ... pt-20">
      <h2>Current selection:</h2>
      {#if selectedAnswer}
        <p>You selected: {selectedAnswer}</p>
        {#if correct === true}
          <p>Your correct!</p>
        {:else if correct === false}
          <p>Your incorrect!</p>
        {/if}
      {/if}
      </div>
      <div class="flex justify-center ... pt-20">
    
        <p class="mr-10">Correct: {correctCount}</p> <p>Incorrect: {wrongCount}</p>
      </div>
  
  
    <div class="flex justify-center ... pt-20">
      <button class="btn btn-primary" on:click= {toggleSettings}>Settings</button>
    </div>
      {#if showSettings}
      
      <form>
        <div class="flex justify-center ... pt-10">
        <fieldset>
          <legend class= "text-lg">Category:</legend>
          <select on:change={handleCategoryChange}>
            <option value="">Any category</option>
            <option value="9">General Knowledge</option>
            <option value="10">Entertainment: Books</option>
            <option value="11">Entertainment: Film</option>
            <option value="12">Entertainment: Music</option>
            <option value="13">Entertainment: comedy</option>
                <option value="14">Entertainment: Television</option>
                <option value="15">Entertainment: Video Games</option>
                <option value="16">Entertainment: Board Games</option>
                <option value="17">Science &amp; Nature</option>
                <option value="18">Science: Computers</option>
                <option value="19">Science: Mathematics</option>
                <option value="20">Mythology</option>
                <option value="21">Sports</option>
                <option value="22">Geography</option>
                <option value="23">History</option>
                <option value="24">Politics</option>
                <option value="25">Art</option>
                <option value="26">Celebrities</option>
                <option value="27">Animals</option>
                <option value="28">Vehicles</option>
                <option value="29">Entertainment: Comics</option>
                <option value="30">Science: Gadgets</option>
                <option value="31">Entertainment: Japanese Anime &amp; Manga</option>
                <option value="32">Entertainment: Cartoon &amp; Animations</option>
              </select>
            </fieldset>
          </div>
          <div class="flex justify-center ... pt-10">
            <fieldset>
              <legend class="text-lg">Difficulty:</legend>
              <select on:change={handleDifficultyChange}>
                <option value="">Any difficulty</option>
                <option value="easy">Easy</option>
                <option value="medium">Medium</option>
                <option value="hard">Hard</option>
              </select>
            </fieldset>
          </div>
          </form>
          {/if}
      
   
      </main>
      
      <style>
        @import '/src/app.css';

      </style>
