from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput

class CalculatorApp(App):
    def build(self):
        self.operators = ['+', '-', '*', '/']
        self.last_was_operator = None
        self.last_button = None
        self.result = TextInput(font_size=32, readonly=True, halign="right", multiline=False)

        layout = BoxLayout(orientation='vertical')
        layout.add_widget(self.result)

        buttons = [
            ['7', '8', '9', '/'],
            ['4', '5', '6', '*'],
            ['1', '2', '3', '-'],
            ['C', '0', '=', '+'],
            ['DEL']  # Added DEL button for single character delete
        ]

        for row in buttons:
            h_layout = BoxLayout()
            for label in row:
                button = Button(text=label, font_size=24)
                button.bind(on_press=self.on_button_press)
                h_layout.add_widget(button)
            layout.add_widget(h_layout)

        return layout

    def on_button_press(self, instance):
        current = self.result.text
        button_text = instance.text

        if button_text == 'C':
            self.result.text = ''
        elif button_text == 'DEL':
            self.result.text = current[:-1]  # Remove last character
        elif button_text == '=':
            try:
                self.result.text = str(eval(current))
            except:
                self.result.text = 'Error'
        else:
            self.result.text += button_text

if __name__ == '__main__':
    CalculatorApp().run()
