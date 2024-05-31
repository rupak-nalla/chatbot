<script setup>
// JavaScript
let i=0;
function generateResponse() {
  
  const responseconversation = document.getElementById('responseconversation');
  i+=1
  const queryDiv = document.getElementById('inputMsg');
  const query = document.getElementById('inputMsg').value;
  queryDiv.value='';
  responseconversation.innerHTML+=`<div id='exchange${i}'><div id='inp${i}'><div id='msg${i}' style="margin-right:10px;">${query}</div><svg xmlns="http://www.w3.org/2000/svg" width="36" height="36" fill="currentColor" class="bi bi-person-circle" viewBox="0 0 16 16"><path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0"/><path fill-rule="evenodd" d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8m8-7a7 7 0 0 0-5.468 11.37C3.242 11.226 4.805 10 8 10s4.757 1.225 5.468 2.37A7 7 0 0 0 8 1"/></svg></div><div id='op${i}'><div id="out${i}"></div></div><div>`
  let inp=document.getElementById(`inp${i}`);
  let msg =document.getElementById(`msg${i}`)
  inp.style.textAlign="right";
  inp.style.display="flex";
  inp.style.justifyContent="right";
  msg.style.backgroundColor='#00b4d8';
  msg.style.width="auto";
  msg.style.padding="10px";
  msg.style.borderRadius="5px";
  const myButton = document.getElementById('generateBtn');
  // const loadingSpinner = document.createElement('div');
  // loadingSpinner.classList.add('spinner');
  
  // myButton.innerHTML = '';
  // myButton.appendChild(loadingSpinner);
  myButton.disabled = true;

  console.log("called");
  fetch('https://api.endpoints.anyscale.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer esecret_um8kn1v9hiumqctegwe93ynevg'
    },
    body: JSON.stringify({
      model: "google/gemma-7b-it",
      messages: [
        { role: "system", content: "You are a helpful assistant." },
        { role: "user", content: query }
      ],
      temperature: 0.1
    })
  })
  .then(response => response.json())
  .then(data => {
    let res = data.choices[0].message.content;
    let op =document.getElementById(`op${i}`)
    op.innerHTML=`<svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="currentColor" class="bi bi-robot" viewBox="0 0 16 16"><path d="M6 12.5a.5.5 0 0 1 .5-.5h3a.5.5 0 0 1 0 1h-3a.5.5 0 0 1-.5-.5M3 8.062C3 6.76 4.235 5.765 5.53 5.886a26.6 26.6 0 0 0 4.94 0C11.765 5.765 13 6.76 13 8.062v1.157a.93.93 0 0 1-.765.935c-.845.147-2.34.346-4.235.346s-3.39-.2-4.235-.346A.93.93 0 0 1 3 9.219zm4.542-.827a.25.25 0 0 0-.217.068l-.92.9a25 25 0 0 1-1.871-.183.25.25 0 0 0-.068.495c.55.076 1.232.149 2.02.193a.25.25 0 0 0 .189-.071l.754-.736.847 1.71a.25.25 0 0 0 .404.062l.932-.97a25 25 0 0 0 1.922-.188.25.25 0 0 0-.068-.495c-.538.074-1.207.145-1.98.189a.25.25 0 0 0-.166.076l-.754.785-.842-1.7a.25.25 0 0 0-.182-.135"/><path d="M8.5 1.866a1 1 0 1 0-1 0V3h-2A4.5 4.5 0 0 0 1 7.5V8a1 1 0 0 0-1 1v2a1 1 0 0 0 1 1v1a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2v-1a1 1 0 0 0 1-1V9a1 1 0 0 0-1-1v-.5A4.5 4.5 0 0 0 10.5 3h-2zM14 7.5V13a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1V7.5A3.5 3.5 0 0 1 5.5 4h5A3.5 3.5 0 0 1 14 7.5"/></svg><div id="out${i}"></div>`
    let out=document.getElementById(`out${i}`)
    out.style.backgroundColor="#00b4d8"
    console.log(res)
    const formattedContent=formatTextToDOM(res);
    out.appendChild(formattedContent);
    out.style.padding="10px";
    out.style.borderRadius="5px";
    out.style.width="auto";
    out.style.margin="10px";
    op.style.textAlign="left";
    op.style.display="flex";
    op.style.justifyContent="left";
    myButton.innerHTML = 'Genrate response';
    myButton.disabled = false;
  })
  .catch(error => {
    console.error(error);
  });
}

let complier=new Map([
        ['**', "h5"],
        [' * ', "br"]
      ])



function formatTextToDOM(text) {
  // Split the text into an array of lines
  const lines = text.split('\n');

  // Create a document fragment to hold the elements
  const fragment = document.createDocumentFragment();

  // Loop through each line
  for (let i = 0; i < lines.length; i++) {
    const line = lines[i].trim();

    // Check if the line starts and ends with "**"
    if (line.startsWith('**') && line.endsWith('**')) {
      // Create a heading element
      const heading = document.createElement('h4');
      heading.textContent = line.slice(2, -2);
      fragment.appendChild(heading);

      // Add a new line element after the heading
      
    } else {
      // Create a paragraph element
      const paragraph = document.createElement('p');
      paragraph.textContent = line;
      fragment.appendChild(paragraph);

      // Add a new line element after the paragraph
      // if (i < lines.length - 1) {
      //   const newLine = document.createElement('br');
      //   fragment.appendChild(newLine);
      // }
    }
  }

  return fragment;
}

// // Example usage
// const inputText = "**This is a heading**\nThis is a new paragraph.\nThis is another paragraph.";
// const formattedContent = formatTextToDOM(inputText);

// // Append the formatted content to the DOM
// const container = document.getElementById('container');
// container.appendChild(formattedContent);


</script>

<template>
  <div id="main">
    <div id="nav">
      <h5 style="margin: 0px;padding: 10px;color: #caf0f8;">ChatBot powerd by Lemma</h5>
    </div>
    <div id="conversation">
      <div id="responseconversation" style="padding-left: 10px;padding-right: 10px;padding-top: 10px;"></div>
      <div id="in">
          <input type="text" class="form-control"  placeholder="enter message" id="inputMsg" >
          <button id="generateBtn" @click="generateResponse" class="btn btn-primary">Generate Response</button>
      </div>
      
    </div>
  </div>



</template>

<style scoped>

.conversation::-webkit-scrollbar {
  width: 8px; /* Width of the scrollbar */
}

.conversation::-webkit-scrollbar-thumb {
  background-color: #888; /* Color of the scrollbar thumb */
  border-radius: 4px; /* Rounded corners of the scrollbar thumb */
}

.conversation::-webkit-scrollbar-track {
  background-color: #f1f1f1; /* Color of the scrollbar track */
}

#conversation{
  background-color: #0077b6;
  padding: 0px 0px 250px 0px;
  height: 100%;
  width: 100%;
  max-height: 100%; /* Set the maximum height of the conversation */
  overflow-y: auto; /* Enable vertical scrolling */
  overflow-x: hidden;
}
#nav{
  background-color: #03045e;
}
#main{
  height: 100%;
  width: 100%;
}
#in{
  position: absolute;
  top:85%;
  right: 20px;
  left: 20px; 
  display: flex;

}
#inputMsg{
  width: 100%;
  margin: 0px 10px 0px 0px;
}
#inp{
  background-color: #edf2f4;
}



</style>