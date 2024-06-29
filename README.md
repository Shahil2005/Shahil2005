<!---
Shahil2005/Shahil2005 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
import tkinter as tk
from tkinter import messagebox
import datetime
import itertools

class Bus:
    def __init__(self, bus_id, source, destination, departure, arrival, seats):
        self.bus_id = bus_id
        self.source = source
        self.destination = destination
        self.departure = departure
        self.arrival = arrival
        self.seats = seats
        self.available_seats = seats

class Ticket:
    def __init__(self, ticket_id, bus, passenger_name):
        self.ticket_id = ticket_id
        self.bus = bus
        self.passenger_name = passenger_name

class BusReservationSystem:
    def __init__(self):
        self.buses = []
        self.tickets = []
        self.ticket_counter = 1

    def add_bus(self, bus):
        self.buses.append(bus)

    def view_buses(self):
        return self.buses

    def book_ticket(self, bus_id, passenger_name):
        bus = next((bus for bus in self.buses if bus.bus_id == bus_id), None)
        if bus:
            if bus.available_seats > 0:
                ticket = Ticket(self.ticket_counter, bus, passenger_name)
                self.tickets.append(ticket)
                bus.available_seats -= 1
                self.ticket_counter += 1
                return f"Ticket booked successfully! Ticket ID: {ticket.ticket_id}"
            else:
                return "Sorry, no available seats on this bus."
        else:
            return "Invalid bus ID."

    def view_tickets(self):
        return self.tickets

def display_fullscreen(window):
    window.attributes("-fullscreen", True)
    window.bind("<Escape>", lambda e: window.attributes("-fullscreen", False))

def show_buses():
    for widget in main_frame.winfo_children():
        widget.destroy()

    buses = system.view_buses()
    if buses:
        for bus in buses:
            bus_info = f"Bus ID: {bus.bus_id}, Source: {bus.source}, Destination: {bus.destination}, Departure: {bus.departure}, Arrival: {bus.arrival}, Available Seats: {bus.available_seats}"
            label = tk.Label(main_frame, text=bus_info, font=("Helvetica", 14), bg="#e1f5fe", fg="#01579b", bd=1, relief=tk.SOLID, padx=10, pady=5)
            label.pack(pady=5, padx=20, fill=tk.X)
    else:
        label = tk.Label(main_frame, text="No buses available", font=("Helvetica", 16), bg="#e1f5fe", fg="#01579b", bd=1, relief=tk.SOLID, padx=10, pady=5)
        label.pack(pady=5)

def book_ticket():
    def submit_booking():
        bus_id = int(bus_id_entry.get())
        passenger_name = passenger_name_entry.get()
        result = system.book_ticket(bus_id, passenger_name)
        messagebox.showinfo("Booking Result", result)

    for widget in main_frame.winfo_children():
        widget.destroy()

    bus_id_label = tk.Label(main_frame, text="Enter Bus ID:", font=("Helvetica", 16), bg="#e1f5fe", fg="#01579b")
    bus_id_label.pack(pady=5)
    bus_id_entry = tk.Entry(main_frame, font=("Helvetica", 16))
    bus_id_entry.pack(pady=5)

    passenger_name_label = tk.Label(main_frame, text="Enter Passenger Name:", font=("Helvetica", 16), bg="#e1f5fe", fg="#01579b")
    passenger_name_label.pack(pady=5)
    passenger_name_entry = tk.Entry(main_frame, font=("Helvetica", 16))
    passenger_name_entry.pack(pady=5)

    submit_button = tk.Button(main_frame, text="Book Ticket", font=("Helvetica", 16), bg="#4CAF50", fg="white", command=submit_booking)
    submit_button.pack(pady=20)
    animate_button(submit_button)

def view_tickets():
    for widget in main_frame.winfo_children():
        widget.destroy()

    tickets = system.view_tickets()
    if tickets:
        for ticket in tickets:
            ticket_info = f"Ticket ID: {ticket.ticket_id}, Bus ID: {ticket.bus.bus_id}, Passenger Name: {ticket.passenger_name}"
            label = tk.Label(main_frame, text=ticket_info, font=("Helvetica", 14), bg="#e1f5fe", fg="#01579b", bd=1, relief=tk.SOLID, padx=10, pady=5)
            label.pack(pady=5, padx=20, fill=tk.X)
    else:
        label = tk.Label(main_frame, text="No tickets booked", font=("Helvetica", 16), bg="#e1f5fe", fg="#01579b", bd=1, relief=tk.SOLID, padx=10, pady=5)
        label.pack(pady=5)

def animate_title():
    current_text = title_label.cget("text")
    new_text = current_text[-1] + current_text[:-1]
    title_label.config(text=new_text)
    root.after(200, animate_title)

def animate_button(button):
    def pulse():
        button.config(bg="#66BB6A")
        button.after(500, lambda: button.config(bg="#4CAF50"))
        button.after(1000, pulse)

    pulse()

def create_gradient_background():
    gradient_frame = tk.Frame(root, width=root.winfo_screenwidth(), height=root.winfo_screenheight())
    gradient_frame.place(x=0, y=0)

    canvas = tk.Canvas(gradient_frame, width=root.winfo_screenwidth(), height=root.winfo_screenheight())
    canvas.pack()

    colors = ["#FFDEE9", "#B5FFFC", "#FFC0CB", "#FFE4E1"]
    steps = 100
    for i in range(steps):
        color = interpolate_color(colors[i % len(colors)], colors[(i + 1) % len(colors)], i / steps)
        canvas.create_rectangle(0, i * root.winfo_screenheight() // steps, root.winfo_screenwidth(), (i + 1) * root.winfo_screenheight() // steps, outline="", fill=color)

def interpolate_color(color1, color2, factor):
    r1, g1, b1 = root.winfo_rgb(color1)
    r2, g2, b2 = root.winfo_rgb(color2)
    r = int(r1 + (r2 - r1) * factor)
    g = int(g1 + (g2 - g1) * factor)
    b = int(b1 + (b2 - b1) * factor)
    return f'#{r // 256:02x}{g // 256:02x}{b // 256:02x}'

def create_color_cycle():
    colors = itertools.cycle(["#FF6F61", "#6B5B95", "#88B04B", "#F7CAC9", "#92A8D1", "#955251", "#B565A7"])
    def change_color():
        next_color = next(colors)
        button_frame.config(bg=next_color)
        root.after(1000, change_color)
    change_color()

# Initialize the reservation system
system = BusReservationSystem()
system.add_bus(Bus(1, "City Coimbatore", "City Chennai", datetime.datetime(2024, 6, 1, 10, 0), datetime.datetime(2024, 6, 1, 14, 0), 40))
system.add_bus(Bus(2, "City Chennai", "City Bangalore", datetime.datetime(2024, 6, 2, 15, 0), datetime.datetime(2024, 6, 2, 20, 0), 30))
system.add_bus(Bus(3, "City Coimbatore", "City Trichy", datetime.datetime(2024, 6, 1, 10, 0), datetime.datetime(2024, 6, 1, 14, 0), 40))
system.add_bus(Bus(4, "City Chennai", "City Dindigul", datetime.datetime(2024, 6, 2, 15, 0), datetime.datetime(2024, 6, 2, 20, 0), 30))

# Initialize the main window
root = tk.Tk()
root.title("Bus Reservation System")
display_fullscreen(root)

# Create gradient background
create_gradient_background()

# Create frames
title_frame = tk.Frame(root, bg="#333")
title_frame.pack(fill=tk.X)

main_frame = tk.Frame(root, bg="#e1f5fe")
main_frame.pack(fill=tk.BOTH, expand=True)

# Title Label with Animation
title_label = tk.Label(title_frame, text="Bus Reservation System", font=("Helvetica", 32, "bold"), fg="white", bg="#333")
title_label.pack(pady=10)
animate_title()

# Centering buttons
button_frame = tk.Frame(root, bg="#e1f5fe")
button_frame.pack(pady=20)

view_buses_button = tk.Button(button_frame, text="View Buses", font=("Helvetica", 16), bg="#2196F3", fg="white", command=show_buses)
view_buses_button.pack(side=tk.LEFT, padx=10)
animate_button(view_buses_button)

book_ticket_button = tk.Button(button_frame, text="Book Ticket", font=("Helvetica", 16), bg="#4CAF50", fg="white", command=book_ticket)
book_ticket_button.pack(side=tk.LEFT, padx=10)
animate_button(book_ticket_button)

view_tickets_button = tk.Button(button_frame, text="View Tickets", font=("Helvetica", 16), bg="#FF5722", fg="white", command=view_tickets)
view_tickets_button.pack(side=tk.LEFT, padx=10)
animate_button(view_tickets_button)

# Center the button frame
button_frame.place(relx=0.5, rely=0.5, anchor=tk.CENTER)

# Start color cycle animation
create_color_cycle()

# Run the main loop
root.mainloop()

--->
