import time
import datetime
from playsound import playsound
import os

class AlarmClock:
    def __init__(self):
        self.alarm_time = None
        self.is_running = False
    
    def set_alarm(self, hours, minutes):
        """Set the alarm time in 24-hour format"""
        if not (0 <= hours < 24 and 0 <= minutes < 60):
            print("Invalid time format. Please use 24-hour format (0-23 hours, 0-59 minutes)")
            return False
        
        self.alarm_time = (hours, minutes)
        print(f"Alarm set for {hours:02d}:{minutes:02d}")
        return True
    
    def display_current_time(self):
        """Display current time"""
        current_time = datetime.datetime.now().strftime("%H:%M:%S")
        return current_time
    
    def check_alarm(self):
        """Check if it's time for the alarm"""
        if self.alarm_time is None:
            return False
        
        current_time = datetime.datetime.now()
        current_hours = current_time.hour
        current_minutes = current_time.minute
        
        return (current_hours, current_minutes) == self.alarm_time
    
    def start(self):
        """Start the alarm clock"""
        if self.alarm_time is None:
            print("Please set an alarm first!")
            return
        
        self.is_running = True
        print(f"Alarm clock started. Current time: {self.display_current_time()}")
        
        try:
            while self.is_running:
                current_time = self.display_current_time()
                print(f"\rCurrent time: {current_time}", end="", flush=True)
                
                if self.check_alarm():
                    self.trigger_alarm()
                    break
                
                time.sleep(1)
        except KeyboardInterrupt:
            print("\nAlarm clock stopped.")
            self.is_running = False
    
    def trigger_alarm(self):
        """Trigger the alarm"""
        print("\n")
        print("🔔 " * 10)
        print("ALARM! ALARM! ALARM!")
        print("🔔 " * 10)
        
        # Play alarm sound (beep)
        for _ in range(10):
            print("\a", end="", flush=True)  # System beep
            time.sleep(0.3)
        
        print("Alarm triggered at", self.display_current_time())
        self.is_running = False
    
    def stop(self):
        """Stop the alarm clock"""
        self.is_running = False
        print("Alarm clock stopped.")


def main():
    """Main function to run the alarm clock"""
    alarm = AlarmClock()
    
    print("=" * 40)
    print("       SIMPLE ALARM CLOCK")
    print("=" * 40)
    print()
    
    while True:
        print("\nOptions:")
        print("1. Set alarm")
        print("2. Start alarm")
        print("3. Show current time")
        print("4. Exit")
        
        choice = input("Enter your choice (1-4): ").strip()
        
        if choice == "1":
            try:
                hours = int(input("Enter hours (0-23): "))
                minutes = int(input("Enter minutes (0-59): "))
                alarm.set_alarm(hours, minutes)
            except ValueError:
                print("Invalid input. Please enter numbers only.")
        
        elif choice == "2":
            alarm.start()
        
        elif choice == "3":
            print(f"Current time: {alarm.display_current_time()}")
        
        elif choice == "4":
            print("Exiting alarm clock. Goodbye!")
            break
        
        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
