# Deckyceasers
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.gridlayout import GridLayout
from kivy.uix.label import Label
from kivy.uix.button import Button

class Calculator(BoxLayout):
    def __init__(self, **kwargs):
        super(Calculator, self).__init__(orientation='vertical', **kwargs)
        self.expression = ''
        
        # Display
        self.display = Label(text='0', font_size=40, size_hint_y=0.2, halign='right', valign='center')
        self.add_widget(self.display)
        
        # Button layout
        button_layout = GridLayout(cols=4, size_hint_y=0.8)
        buttons = [
            ['7', '8', '9', '/'],
            ['4', '5', '6', '*'],
            ['1', '2', '3', '-'],
            ['0', '.', '=', '+'],
            ['C', 'H', '', '']
        ]
        
        for row in buttons:
            for symbol in row:
                if symbol:
                    btn = Button(text=symbol, font_size=30)
                    btn.bind(on_press=lambda instance, s=symbol: self.press(s))
                else:
                    btn = Button(disabled=True)  # Empty space
                button_layout.add_widget(btn)
        
        self.add_widget(button_layout)
    
    def press(self, symbol):
        if symbol == '=':
            try:
                result = str(eval(self.expression))
                self.display.text = result
                self.expression = result
            except:
                self.display.text = 'Error'
                self.expression = ''
        elif symbol == 'C':
            self.expression = ''
            self.display.text = '0'
        elif symbol == 'H':
            App.get_running_app().stop()  # Close the app
        else:
            self.expression += str(symbol)
            self.display.text = self.expression if self.expression else '0'

class CalculatorApp(App):
    def build(self):
        return Calculator()

if __name__ == '__main__':
    CalculatorApp().run()   
