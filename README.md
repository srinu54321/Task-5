import tkinter as tk
from tkinter import messagebox

class QuizApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Quiz Game")

        # Initialize quiz questions and answers
        self.questions = [
            {
                "question": "What is the capital of France?",
                "options": ["London", "Berlin", "Paris", "Madrid"],
                "correct_answer": "Paris"
            },
            {
                "question": "Which planet is known as the Red Planet?",
                "options": ["Earth", "Mars", "Venus", "Jupiter"],
                "correct_answer": "Mars"
            },
            {
                "question": "What is 7 + 3?",
                "options": ["10", "12", "14", "8"],
                "correct_answer": "10"
            },
        ]

        self.current_question = 0
        self.score = 0

        # Create and configure widgets
        self.question_label = tk.Label(root, text="")
        self.question_label.pack()

        self.option_buttons = []
        for i in range(4):
            button = tk.Button(root, text="", command=lambda i=i: self.check_answer(i))
            button.pack()
            self.option_buttons.append(button)

        self.feedback_label = tk.Label(root, text="")
        self.feedback_label.pack()

        self.next_button = tk.Button(root, text="Next Question", command=self.next_question)
        self.next_button.pack()

        self.final_score_label = tk.Label(root, text="")
        self.final_score_label.pack()

        self.load_question(self.current_question)

    def load_question(self, question_num):
        if question_num < len(self.questions):
            question_data = self.questions[question_num]
            self.question_label.config(text=question_data["question"])
            for i, option in enumerate(question_data["options"]):
                self.option_buttons[i].config(text=option)
        else:
            self.show_final_score()

    def check_answer(self, selected_option):
        question_data = self.questions[self.current_question]
        correct_answer = question_data["correct_answer"]

        if question_data["options"][selected_option] == correct_answer:
            self.score += 1
            self.feedback_label.config(text="Correct!")
        else:
            self.feedback_label.config(text=f"Wrong. Correct answer: {correct_answer}")

    def next_question(self):
        self.current_question += 1
        self.load_question(self.current_question)
        self.feedback_label.config(text="")

    def show_final_score(self):
        self.question_label.config(text="")
        for button in self.option_buttons:
            button.config(state=tk.DISABLED)
        self.next_button.config(state=tk.DISABLED)

        self.final_score_label.config(text=f"Final Score: {self.score}/{len(self.questions)}")
        messagebox.showinfo("Quiz Complete", f"Your Score: {self.score}/{len(self.questions)}")

if __name__ == "__main__":
    root = tk.Tk()
    app = QuizApp(root)
    root.mainloop()
