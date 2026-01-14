# qr-logo-generator

```python
# copy from https://www.jianshu.com/p/c0073c6aa544

import qrcode
from PIL import Image
# import matplotlib.pyplot as plt

def getQRcode(data, output_file, input_file, logo_size=1/3):
    qr = qrcode.QRCode(
        version=1,
        error_correction=qrcode.constants.ERROR_CORRECT_H,
        box_size=50,
        border=4,
    )

    qr.add_data(data)
    qr.make(fit=True)
    img = qr.make_image(fill_color="#000000", back_color="#FFFFFF00")

    # add logo
    icon = Image.open(input_file)
    img_w, img_h = img.size
    size_w = int(img_w * logo_size)
    size_h = int(img_h * logo_size)
    icon_w, icon_h = icon.size
    if icon_w > size_w:
        icon_w = size_w
    if icon_h > size_h:
        icon_h = size_h
    icon = icon.resize((icon_w, icon_h), Image.ANTIALIAS)

    w = int((img_w - icon_w) / 2)
    h = int((img_h - icon_h) / 2)

    img.paste(icon, (w, h))

    # plt.imshow(img)
    # plt.show()

    img = img.convert("RGBA")
    img_data = img.getdata()
    new_img_data = [(r, g, b, 0) if (r, g, b, a) == (255, 255, 255, 0) else (r, g, b, a) for r, g, b, a in img_data]
    img.putdata(new_img_data)
    img.save(output_file, format='PNG')

    return img

if __name__ == '__main__':
    # PREP-NexT
    getQRcode("https://github.com/PREP-NexT", '300_white_cropped-01_qrcode.png', "300_white_cropped-01.png")
    # PREP-SHOT
    getQRcode("https://prep-next.github.io/PREP-SHOT/", '300_crop_white-02.png', "300_crop_white-02.png")
```
