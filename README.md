// Function to simulate a user click
function simulateClick(element) {
    const event = new MouseEvent('click', {
        view: window,
        bubbles: true,
        cancelable: true
    });
    element.dispatchEvent(event);
}

// Main cheat function
const cheat = async () => {
    const bodyDiv = document.querySelector("body>div");
    const reactRoot = (function react(r = bodyDiv) {
        return Object.values(r)[1]?.children?.[0]?._owner?.stateNode ? r : react(r.querySelector(":scope>div"));
    })();

    const stateNode = Object.values(reactRoot)[1].children[0]._owner.stateNode;
    const Question = stateNode.state.question || stateNode.props.client.question;

    let ind = 0;
    while (ind < Question.answers.length) {
        let found = false;
        for (let j = 0; j < Question.correctAnswers.length; j++) {
            if (Question.answers[ind] === Question.correctAnswers[j]) {
                found = true;
                break;
            }
        }
        ind++;
        const answerElement = document.querySelector("[class*='answersHolder'] :nth-child(" + ind + ") > div");
        if (found) {
            answerElement.style.backgroundColor = "rgb(0, 207, 119)";
            simulateClick(answerElement);
            console.log(`Clicked correct answer at index ${ind - 1}`);
        } else {
            answerElement.style.backgroundColor = "rgb(189, 15, 38)";
        }
        // Add a short delay to allow time for processing (10 milliseconds)
        await new Promise(resolve => setTimeout(resolve, 10));
    }
};

// Add event listener to trigger cheat function when "a" key is pressed
document.addEventListener("keydown", function(event) {
    if (event.key === "a") {
        console.log("Key 'a' pressed. Running cheat function...");
        cheat();
    }
});
