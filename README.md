
# 🔍 Desafio DIO - Criação de base e treinamento da rede YOLO

Este projeto realiza o **treinamento de um modelo YOLOv3** usando imagens do dataset [COCO](https://cocodataset.org/#home), com **transferência de aprendizado** a partir dos pesos pré-treinados. Novas classes, como `capacete` e `máscara`, são adicionadas ao conjunto original.

A proposta desse projeto é um desafio do Bootcamp BairesDev - Machine Learning Training da [DIO](https://www.dio.me/).

---

## 📂 Estrutura do Projeto

```
├── coco/                      # Diretório clonado com as anotações do COCO
│   ├── annotations/           # Arquivos JSON de anotação
│   └── val2017/               # Conjunto de imagens
├── darknet/                   # Código-fonte da framework YOLO (Darknet)
│   ├── cfg/                   # Arquivos de configuração dos modelos
│   ├── data/                  # Arquivos de classes, train.txt, etc.
│   └── yolov3.weights         # Pesos pré-treinados
├── scripts/                   # Scripts auxiliares
│   └── convert_coco_to_yolo.py
├── train.txt                  # Caminhos absolutos das imagens de treino
├── yolov3-custom.cfg          # Configuração customizada do modelo
└── README.md
```

---

## 📦 Requisitos

- Google Colab (com GPU)
- Python ≥ 3.7
- Bibliotecas:
  - `opencv-python`
  - `tqdm`
  - `matplotlib`
  - `numpy`

Instale com:

```bash
pip install opencv-python tqdm matplotlib
```

---

## 🧠 Etapas do Projeto

### 1. 🔽 Baixar Dataset COCO

```bash
# Anotações e imagens
wget http://images.cocodataset.org/zips/val2017.zip
wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip

unzip val2017.zip -d data/
unzip annotations_trainval2017.zip -d data/
```

### 2. 🛠️ Converter Anotações para Formato YOLO

Use o script `convert_coco_to_yolo.py` para converter `instances_val2017.json` para arquivos `.txt`:

```bash
python scripts/convert_coco_to_yolo.py
```

### 3. 📝 Criar arquivos auxiliares

- `train.txt`: lista com caminhos absolutos das imagens
- `obj.names`: nomes das 81 classes (incluindo novas)
- `obj.data`: arquivo de configuração do dataset

### 4. ⚙️ Configurar `yolov3-custom.cfg`

Adapte o arquivo `yolov3.cfg`:

- `classes=81` (nas seções `[yolo]`)
- `filters=(classes + 5) * 3 = 258` (nas camadas anteriores às seções `[yolo]`)
- Reduza `max_batches`, `steps`, `batch`, `subdivisions` se quiser treinar rapidamente.

---

## 🚀 Iniciar Treinamento

```bash
!./darknet/darknet detector train data/obj.data cfg/yolov3-custom.cfg yolov3.weights -dont_show -map
```

> O treinamento pode ser acompanhado pela métrica de `mAP` e `loss` no console.

---

## 🖼️ Realizar Inferência

```bash
!./darknet/darknet detector test data/obj.data cfg/yolov3-custom.cfg backup/yolov3-custom_final.weights data/val2017/exemplo.jpg
```

Resultado será salvo como `predictions.jpg`.

---

## 📈 Resultados Esperados

- Precisão aumentada para classes novas com poucas amostras
- Detecção em imagens reais com `capacete`, `máscara`, etc.
- Uso eficiente da arquitetura YOLOv3 com transfer learning

---

## 📌 Observações

- Se estiver no Google Colab, certifique-se de **ativar GPU** em `Ambiente de execução > Alterar tipo de hardware`.
- O erro `CUDA version higher than driver version` é um *warning*, e geralmente não impede o treinamento no Colab.

---

## 📚 Referências

- [YOLOv3 Paper](https://pjreddie.com/media/files/papers/YOLOv3.pdf)
- [COCO Dataset](https://cocodataset.org/#home)
- [Darknet GitHub](https://github.com/AlexeyAB/darknet)

---

## 🧑‍💻 Autor

**Valci Júnior**  
Cientista de Dados | Deep Learning Enthusiast  
[LinkedIn](https://www.linkedin.com/in/valci-junior/) · [GitHub](https://github.com/V4lciJr)
