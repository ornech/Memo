# Etendre un FS

```bash
lsblk
```

Sortie pertinente :

```
sda                         150G
├─sda1                      1M
├─sda2                      2G      → monté sur /boot
└─sda3                      48G     → utilisé par LVM
  └─ubuntu--vg-ubuntu--lv  24G     → monté sur /
```

### Ouvrir `parted` pour étendre `sda3`

```bash
sudo parted /dev/sda
```

Dans `parted` :

```bash
print
```

---

### Redimensionner la partition `sda3`

Toujours dans `parted` :

```bash
resizepart 3
161GB
```

Puis :
```bash
quit
```

---

### Informer le système du changement de table de partitions

```bash
sudo partprobe
```

---

### Étendre le volume physique LVM (qui repose sur `/dev/sda3`)

```bash
sudo pvresize /dev/sda3
```

Cela permet à LVM de voir que la partition physique a été agrandie.

---

### Étendre le volume logique `ubuntu-lv` à tout l’espace disponible

```bash
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

---

### Redimensionner le système de fichiers `ext4`

```bash
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

---

### 📊 7. Vérifier

```bash
df -h
```
