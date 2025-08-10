# Clock
import tkinter as tk
from datetime import datetime
import time
import threading

class ClockStopwatchApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Clock + Stopwatch")
        self.root.geometry("350x250")

        # Clock display
        self.clock_label = tk.Label(root, text="", font=("Helvetica", 20), fg="blue")
        self.clock_label.pack(pady=10)
        self.update_clock()

        # Stopwatch display
        self.stopwatch_label = tk.Label(root, text="00:00:00", font=("Helvetica", 30), fg="green")
        self.stopwatch_label.pack(pady=10)

        # Buttons
        self.start_btn = tk.Button(root, text="Start", width=10, command=self.start_stopwatch)
        self.start_btn.pack(side=tk.LEFT, padx=10, pady=10)

        self.stop_btn = tk.Button(root, text="Stop", width=10, command=self.stop_stopwatch)
        self.stop_btn.pack(side=tk.LEFT, padx=10, pady=10)

        self.reset_btn = tk.Button(root, text="Reset", width=10, command=self.reset_stopwatch)
        self.reset_btn.pack(side=tk.LEFT, padx=10, pady=10)

        # Stopwatch state
        self.running = False
        self.start_time = None
        self.elapsed_time = 0

    def update_clock(self):
        now = datetime.now().strftime("%H:%M:%S")
        self.clock_label.config(text="Clock: " + now)
        self.root.after(1000, self.update_clock)

    def update_stopwatch(self):
        while self.running:
            time.sleep(0.1)
            self.elapsed_time = time.time() - self.start_time
            minutes, seconds = divmod(int(self.elapsed_time), 60)
            hours, minutes = divmod(minutes, 60)
            time_str = f"{hours:02}:{minutes:02}:{seconds:02}"
            self.stopwatch_label.config(text=time_str)

    def start_stopwatch(self):
        if not self.running:
            self.running = True
            self.start_time = time.time() - self.elapsed_time
            threading.Thread(target=self.update_stopwatch, daemon=True).start()

    def stop_stopwatch(self):
        if self.running:
            self.running = False

    def reset_stopwatch(self):
        self.running = False
        self.elapsed_time = 0
        self.stopwatch_label.config(text="00:00:00")


if __name__ == "__main__":
    root = tk.Tk()
    app = ClockStopwatchApp(root)
    root.mainloop()
