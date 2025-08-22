# Dialogs

* Yes = True
* No = False
* Cancel = None

## Yes/No dialog

```python

        dlg = messagebox.askyesno(
            'Delete project',
            f'Are you sure you want to delete {project.name}?',
            parent=self.root,
        )
        if dlg is False:
            return
```
