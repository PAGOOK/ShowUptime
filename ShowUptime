#Python 3.13.12
import pystray
from pystray import MenuItem as MenuItem, Menu
import ctypes
import socket
import psutil
import threading
from datetime import datetime
from PIL import Image, ImageDraw
import time
import sys
from tendo import singleton

try:
    me = singleton.SingleInstance()
except singleton.SingleInstanceException:
    MessageBox = ctypes.windll.user32.MessageBoxW
    MessageBox(None, "ShowUptime has already started", "ShowUptime", 0)
    sys.exit(-1)

try:
    aktuelle_systemzeit = 0
    def TrayIconGreen():
        image = Image.new("RGBA", (200, 200))
        draw = ImageDraw.Draw(image)
        draw.ellipse((20, 20, 180, 180), fill = "limegreen", outline = "slategrey", width=10)
        return image

    def start_notification(TrayIconGreen):
        ShowUptime.visible = True
        ShowUptime.notify("ShowUptime started", title=" ")

    def update_label(ShowUptime):
        global aktuelle_systemzeit
        while True:
            uptime_sekunden = datetime.now() - datetime.fromtimestamp(psutil.boot_time())
            days = uptime_sekunden.days
            hours, rem = divmod(uptime_sekunden.seconds, 3600)
            minutes, seconds = divmod(rem, 60)
            time.sleep(1)
            ShowUptime.title = f"["+socket.gethostname()+"]\n"+f"Uptime: {days} Days, {hours} Hours, {minutes} Minutes, {seconds} Seconds"

    def uptime_label(MenuItem):
        ShowUptime.title = f"{aktuelle_systemzeit}"

    main_menu = (MenuItem("ShowUptime [Version 1.0 by P A G O O K]", lambda: None),
    pystray.Menu.SEPARATOR,
    MenuItem("Exit", lambda ShowUptime: ShowUptime.stop()))
    ShowUptime = pystray.Icon("ShowUptimeIcon", TrayIconGreen(), "ShowUptime", main_menu)

    thread = threading.Thread(target=update_label, args=(ShowUptime,), daemon=True)
    thread.start()
    ShowUptime.run(setup=start_notification)
except Exception as e:
    MessageBox = ctypes.windll.user32.MessageBoxW
    MessageBox(None, "An unexpected error has occurred.\nShowUptime is closing.", "ShowUptime", 0)
