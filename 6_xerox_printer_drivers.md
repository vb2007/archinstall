# Intalling & configuring drivers for Xerox network printers

The following guide should be suiteable for most Xerox network printers. I've only tested it with **WorkCentre 3025**.

## Installing drivers & CUPS

Download [THIS](https://aur.archlinux.org/packages/xerox-spl-driver-printer) package from the AUR. It should also install CUPS as it's dependency.

Build it manually or use yay:

```shell
yay -S xerox-spl-driver-printer cups
```

After installation, start & enable CUPS:

```shell
sudo systemctl start cups
```

```shell
sudo systemctl enable cups
```

The sys group owns the CUPS config, so add your user to it (if haven't already) to avoid entering passwords on every step:

```shell
sudo usermod -aG sys $USER
```

## Configuring CUPS

1. Open the CUPS local web server at [http://localhost:631](http://localhost:631)
2. Click on the **Administration** tab at the top
3. Enter your username & password (`root` & `your_root_password`)
4. Select **Add Printer**
5. Select your printer from the list
  - If it shows up multiple times - like in my case - select the one that has the printer's local IPv4 address displayed and uses the `socket://` protocoll.
6. Customize the printing options if you would like (margins, paper size, etc.), and finally test printing from your browser, LibreOffice, etc.
