# Images

## Simple image

```python

from PIL import ImageTk, Image
```

```python

    img = Image.open(file)
    img = ImageTk.PhotoImage(img)
    label = ttk.Label(self.thumbnails, image=img)
    label.image = image_1  # Keep a reference - important!!!!
    label.grid(row=0, column=column)
```

## Scale an image

```python

    img = Image.open(file)
    scale = 100 / img.height
    img = img.resize(
        (int(img.width * scale), int(img.height * scale)),
        Image.LANCZOS)
    img = ImageTk.PhotoImage(img)
```

## Image from API

*image_str* is a string of the form:

> 'data:image/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAeCAYAAAA7MK6iAAABhGlDQ1BJQ0MgUHJvZmlsZQAAeJx9kT1Iw0AYht+mSqVUOthBxCFDdbIgKuKoVShChVArtOpgcukfNGlIUlwcBdeCgz+LVQcXZ1 ...

```python

def image_from_api(image_str: str) -> Image:
    if ',' in image_str:
        # Remove 'data:image/jpeg;base64,'
        image_str = image_str.split(',')[1]

    missing_padding = len(image_str) % 4
    if missing_padding:
        image_str += '=' * (4 - missing_padding)

    image_data = base64.b64decode(image_str)
    image = Image.open(BytesIO(image_data))
    return image
```
