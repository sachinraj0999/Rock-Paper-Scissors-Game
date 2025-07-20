# Rock-Paper-Scissors-Game
# project 3
import tkinter as tk
import random
from datetime import datetime

# Global variables
user_score = 0
comp_score = 0
choices = ["Rock", "Paper", "Scissors"]
emojis = {"Rock": "🪨", "Paper": "📄", "Scissors": "✂️"}

# Game logic
def play(user_choice):
    global user_score, comp_score

    comp_choice = random.choice(choices)

    if user_choice == comp_choice:
        result = "It's a Draw 🤝"
        color = "blue"
    elif (user_choice == "Rock" and comp_choice == "Scissors") or \
         (user_choice == "Paper" and comp_choice == "Rock") or \
         (user_choice == "Scissors" and comp_choice == "Paper"):
        result = "You Win! 🎉"
        color = "green"
        user_score += 1
    else:
        result = "You Lose! 😢"
        color = "red"
        comp_score += 1

    result_label.config(
        text=f"You chose: {emojis[user_choice]} {user_choice}\n"
             f"Computer chose: {emojis[comp_choice]} {comp_choice}\n\n{result}",
        fg=color
    )
    score_label.config(text=f"Your Score: {user_score}  |  Computer Score: {comp_score}")
    save_to_file(user_choice, comp_choice, result)

# Reset game
def reset_game():
    global user_score, comp_score
    user_score = 0
    comp_score = 0
    result_label.config(text="Game Reset! Choose Rock, Paper or Scissors.", fg="black")
    score_label.config(text="Your Score: 0  |  Computer Score: 0")

# Save result to file
def save_to_file(user, comp, result):
    with open("game_results.txt", "a") as f:
        now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        f.write(f"{now} | You: {user}, Computer: {comp}, Result: {result}\n")

# GUI setup
root = tk.Tk()
root.title("Rock-Paper-Scissors Game")
root.geometry("500x480")
root.resizable(False, False)
root.config(bg="#f0f0f0")

# Title
tk.Label(root, text="🎮 Rock-Paper-Scissors Game", font=("Helvetica", 18, "bold"), bg="#f0f0f0").pack(pady=15)

# Score
score_label = tk.Label(root, text="Your Score: 0  |  Computer Score: 0", font=("Arial", 12), bg="#f0f0f0")
score_label.pack()

# Result
result_label = tk.Label(root, text="Choose Rock, Paper or Scissors to Start", font=("Arial", 13), bg="#f0f0f0", fg="black", justify="center")
result_label.pack(pady=20)

# Buttons
button_frame = tk.Frame(root, bg="#f0f0f0")
button_frame.pack()

def make_button(name):
    return tk.Button(button_frame, text=f"{emojis[name]} {name}", font=("Arial", 12), width=15, command=lambda: play(name))

make_button("Rock").grid(row=0, column=0, padx=10, pady=10)
make_button("Paper").grid(row=0, column=1, padx=10, pady=10)
make_button("Scissors").grid(row=0, column=2, padx=10, pady=10)

# Reset & Exit
tk.Button(root, text="🔁 Reset", font=("Arial", 11), bg="#FFA726", fg="white", width=15, command=reset_game).pack(pady=10)
tk.Button(root, text="❌ Exit", font=("Arial", 11), bg="red", fg="white", width=15, command=root.destroy).pack()

# Run GUI
root.mainloop()
