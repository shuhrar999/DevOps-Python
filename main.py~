import sys
from PyQt5.QtWidgets import (QApplication, QMainWindow, QWidget, QVBoxLayout, QHBoxLayout,
                             QLabel, QLineEdit, QPushButton, QCheckBox, QComboBox,
                             QDateTimeEdit, QDial, QDoubleSpinBox,
                             QFontComboBox, QLCDNumber, QProgressBar, QRadioButton,
                             QSlider, QSpinBox, QTimeEdit, QListWidget, QMessageBox)
from PyQt5.QtCore import Qt, QDateTime, QTimer


class ReminderApp(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Простое приложение для напоминаний")
        self.setGeometry(100, 100, 600, 400)

        # Центральный виджет
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        main_layout = QHBoxLayout(central_widget)

        # Левая панель - создание напоминания
        left_panel = QWidget()
        left_layout = QVBoxLayout(left_panel)

        # QLineEdit для текста напоминания
        self.reminder_text = QLineEdit()
        self.reminder_text.setPlaceholderText("Введите текст напоминания")
        left_layout.addWidget(QLabel("Текст напоминания:"))
        left_layout.addWidget(self.reminder_text)

        # QDateTimeEdit для даты и времени
        self.reminder_datetime = QDateTimeEdit()
        self.reminder_datetime.setDateTime(QDateTime.currentDateTime().addSecs(3600))
        self.reminder_datetime.setCalendarPopup(True)
        left_layout.addWidget(QLabel("Дата и время:"))
        left_layout.addWidget(self.reminder_datetime)

        # QComboBox для категории
        self.reminder_category = QComboBox()
        self.reminder_category.addItems(["Личное", "Работа", "Семья", "Друзья", "Другое"])
        left_layout.addWidget(QLabel("Категория:"))
        left_layout.addWidget(self.reminder_category)

        # QSpinBox для приоритета
        self.reminder_priority = QSpinBox()
        self.reminder_priority.setRange(1, 5)
        self.reminder_priority.setValue(3)
        left_layout.addWidget(QLabel("Приоритет (1-5):"))
        left_layout.addWidget(self.reminder_priority)

        # QCheckBox для повторения
        self.reminder_repeat = QCheckBox("Повторяющееся напоминание")
        left_layout.addWidget(self.reminder_repeat)

        # QRadioButton для типа напоминания
        left_layout.addWidget(QLabel("Тип напоминания:"))
        self.reminder_type_notification = QRadioButton("Уведомление")
        self.reminder_type_alarm = QRadioButton("Будильник")
        self.reminder_type_notification.setChecked(True)
        left_layout.addWidget(self.reminder_type_notification)
        left_layout.addWidget(self.reminder_type_alarm)

        # QDoubleSpinBox для громкости (для будильника)
        self.reminder_volume = QDoubleSpinBox()
        self.reminder_volume.setRange(0.1, 1.0)
        self.reminder_volume.setSingleStep(0.1)
        self.reminder_volume.setValue(0.5)
        left_layout.addWidget(QLabel("Громкость:"))
        left_layout.addWidget(self.reminder_volume)

        # QPushButton для добавления напоминания
        add_button = QPushButton("Добавить напоминание")
        add_button.clicked.connect(self.add_reminder)
        left_layout.addWidget(add_button)

        # QDial для демонстрации (регулировка чего-либо)
        self.demo_dial = QDial()
        self.demo_dial.setRange(0, 100)
        self.demo_dial.setValue(50)
        left_layout.addWidget(QLabel("Регулятор:"))
        left_layout.addWidget(self.demo_dial)

        # QSlider для демонстрации
        self.demo_slider = QSlider(Qt.Horizontal)
        self.demo_slider.setRange(0, 100)
        self.demo_slider.setValue(30)
        left_layout.addWidget(QLabel("Прогресс:"))
        left_layout.addWidget(self.demo_slider)

        # QFontComboBox для выбора шрифта
        self.font_selector = QFontComboBox()
        left_layout.addWidget(QLabel("Шрифт:"))
        left_layout.addWidget(self.font_selector)

        # QLCDNumber для демонстрации
        self.demo_lcd = QLCDNumber()
        self.demo_lcd.display(123)
        left_layout.addWidget(QLabel("LCD дисплей:"))
        left_layout.addWidget(self.demo_lcd)

        # QProgressBar для демонстрации
        self.demo_progress = QProgressBar()
        self.demo_progress.setValue(45)
        left_layout.addWidget(QLabel("Прогресс бар:"))
        left_layout.addWidget(self.demo_progress)

        # QTimeEdit для демонстрации
        self.demo_time = QTimeEdit()
        self.demo_time.setTime(QDateTime.currentDateTime().time())
        left_layout.addWidget(QLabel("Время:"))
        left_layout.addWidget(self.demo_time)

        # Правая панель - список напоминаний
        right_panel = QWidget()
        right_layout = QVBoxLayout(right_panel)

        # QListWidget для отображения напоминаний
        self.reminders_list = QListWidget()
        right_layout.addWidget(QLabel("Список напоминаний:"))
        right_layout.addWidget(self.reminders_list)

        # Кнопки управления
        button_layout = QHBoxLayout()
        delete_button = QPushButton("Удалить")
        delete_button.clicked.connect(self.delete_reminder)
        button_layout.addWidget(delete_button)

        clear_button = QPushButton("Очистить все")
        clear_button.clicked.connect(self.clear_reminders)
        button_layout.addWidget(clear_button)

        right_layout.addLayout(button_layout)

        # Добавляем панели в главный макет
        main_layout.addWidget(left_panel, 2)
        main_layout.addWidget(right_panel, 1)

        # Таймер для проверки напоминаний
        self.timer = QTimer(self)
        self.timer.timeout.connect(self.check_reminders)
        self.timer.start(1000)  # Проверка каждую секунду

        # Список напоминаний
        self.reminders = []

    def add_reminder(self):
        text = self.reminder_text.text()
        if not text:
            QMessageBox.warning(self, "Ошибка", "Введите текст напоминания!")
            return

        reminder = {
            "text": text,
            "datetime": self.reminder_datetime.dateTime(),
            "category": self.reminder_category.currentText(),
            "priority": self.reminder_priority.value(),
            "repeat": self.reminder_repeat.isChecked(),
            "type": "notification" if self.reminder_type_notification.isChecked() else "alarm",
            "volume": self.reminder_volume.value(),
            "active": True
        }

        self.reminders.append(reminder)
        self.update_reminders_list()
        self.reminder_text.clear()

        QMessageBox.information(self, "Успех", "Напоминание добавлено!")

    def delete_reminder(self):
        current_item = self.reminders_list.currentItem()
        if current_item:
            index = self.reminders_list.row(current_item)
            del self.reminders[index]
            self.update_reminders_list()

    def clear_reminders(self):
        self.reminders = []
        self.update_reminders_list()

    def update_reminders_list(self):
        self.reminders_list.clear()
        for reminder in self.reminders:
            dt = reminder["datetime"].toString("dd.MM.yyyy hh:mm")
            item_text = f"{dt} - {reminder['text']} ({reminder['category']}, приоритет: {reminder['priority']})"
            self.reminders_list.addItem(item_text)

    def check_reminders(self):
        current_datetime = QDateTime.currentDateTime()

        for reminder in self.reminders:
            if reminder["active"] and current_datetime >= reminder["datetime"]:
                self.show_reminder(reminder)
                if reminder["repeat"]:
                    # Если повторяющееся, обновляем дату на следующую неделю
                    reminder["datetime"] = reminder["datetime"].addDays(7)
                else:
                    reminder["active"] = False

        self.update_reminders_list()

    def show_reminder(self, reminder):
        msg = QMessageBox(self)
        msg.setWindowTitle("Напоминание")
        msg.setText(reminder["text"])
        msg.setInformativeText(f"Категория: {reminder['category']}\nПриоритет: {reminder['priority']}")

        if reminder["type"] == "alarm":
            msg.setIcon(QMessageBox.Warning)
            msg.setStandardButtons(QMessageBox.Ok)
        else:
            msg.setIcon(QMessageBox.Information)
            msg.setStandardButtons(QMessageBox.Ok)

        msg.exec_()


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = ReminderApp()
    window.show()
    sys.exit(app.exec_())