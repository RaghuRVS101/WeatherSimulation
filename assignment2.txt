from numpy import random

class WeatherSimulation:

    def __init__(self, transition_probabilities, holding_times):
        self.transition_probabilities = transition_probabilities
        self.holding_times = holding_times
        self.state = 'sunny'
        self.available_states = [i for i in transition_probabilities.keys()]
        for i in transition_probabilities.values():
            value_list = [r for r in i.values()]
            if sum(value_list) != 1:
                raise RuntimeError("RuntimeError") #raises runtime error if sum not equal to 1
        self.remaining_hours = 0
        self.dictionary = {key:0 for key in self.transition_probabilities.keys()}
        self.percentage_list = []
    
    # returns a list of all states
    def get_states(self):
        return self.available_states
    
    # returns the current state
    def current_state(self):
        return self.state
    #  moves to the next state in simulation

    def next_state(self):
        if self.remaining_hours <= 0 :
            self.state = random.choice(
                list(self.transition_probabilities.keys()), p=list(self.transition_probabilities[self.state].values()))
            self.remaining_hours = self.holding_times[self.state]
            self.remaining_hours -= 1
        else:
            self.remaining_hours -= 1
            
            
            
    # changes the current state to the new_state
    def set_state(self, new_state):
        self.state = new_state
        

    def iterable(self):
        while(1) :
            yield self.state
            self.next_state()
            
    # returns the remaining hours in the current state
    def current_state_remaining_hours(self):
        return self.remaining_hours

    # runs the simulation for the amount of hours and returns a list of percentages
    def simulate(self, hours):
        for i in range(hours):
            self.dictionary[self.state] += 1
            self.next_state()
        for values in self.dictionary.values():
            percentage = (values/hours)*100
            self.percentage_list.append(percentage)
        return self.percentage_list
        