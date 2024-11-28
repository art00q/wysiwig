<template>
  <div class="editor-container">
    <div class="toolbar">
      <button class="button" @click="undo" title="Назад">
        <img src="../assets/back.svg" alt="back">
      </button>
      <button class="button" @click="redo" title="Вперёд">
        <img src="../assets/next.svg" alt="next">
      </button>
      <button class="button" @click="setHeading" title="Заголовок">
        <img src="../assets/paragraph.svg" alt="H">
      </button>
      <button class="button" @click="setParagraph" title="Абзац">
        <img src="../assets/header.svg" alt="P">
      </button>
      <button class="button" @click="insertImage" title="Вставить картинку">
        <img src="../assets/img.svg" alt="img">
      </button>
      <button class="copy" @click="copyHtml" title="Копировать HTML">
        Скопировать HTML
      </button>
    </div>

    <div
      class="editor"
      contenteditable="true"
      ref="editor"
      @input="saveState"
      @keydown="handleKeydown"
    >
      <p>Текст для редактирования...</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      history: [],
      historyIndex: -1,
    };
  },
  methods: {
    saveState() {
      const html = this.$refs.editor.innerHTML;
      if (this.historyIndex < this.history.length - 1) {
        this.history = this.history.slice(0, this.historyIndex + 1);
      }
      this.history.push(html);
      this.historyIndex++;
    },
    undo() {
      if (this.historyIndex > 0) {
        this.historyIndex--;
        this.$refs.editor.innerHTML = this.history[this.historyIndex];
      }
    },
    redo() {
      if (this.historyIndex < this.history.length - 1) {
        this.historyIndex++;
        this.$refs.editor.innerHTML = this.history[this.historyIndex];
      }
    },
    setHeading() {
      const editor = this.$refs.editor;
      if (editor) {
        document.execCommand("formatBlock", false, "h1");
      }
    },
    setParagraph() {
      const editor = this.$refs.editor;
      if (editor) {
        document.execCommand("formatBlock", false, "p");
      }
    },
    insertImage() {
      const editor = this.$refs.editor;
      const imageUrl = prompt("Введите URL изображения:");
      if (editor && imageUrl && imageUrl.trim() !== "") {
        const imgTag = `<img src="${imageUrl}" alt="image" class="editor-img" />`;
        editor.innerHTML += imgTag;
      } else {
        alert("Пожалуйста, введите корректный URL изображения.");
      }
    },
    copyHtml() {
      const html = this.$refs.editor.innerHTML;
      navigator.clipboard.writeText(html).then(() => {
        alert("HTML скопирован в буфер обмена!");
      });
    },
    handleKeydown(event) {
      if (event.key === "Enter") {
        this.saveState();
      }
    }
  },
  mounted() {
    this.saveState();
  }
};
</script>

<style scoped>
.editor-container {
  width: 80%;
  margin: 20px auto;
  background-color: #181818;
  color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.toolbar {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
  justify-content: flex-start;
}

.button {
  width: 42px;
  height: 38px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(40, 40, 40, 1);
  border: none;
  font-size: 18px;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.3s;
}

.button:hover {
  background-color: #333;
}

.button:focus {
  outline: none;
}

.button img {
  max-width: 22px;
  max-height: 22px;
}

.copy {
  background-color: transparent;
  border: none;
  color: rgba(99, 158, 255, 1);
  font-size: 15px;
}

.icon {
  font-size: 18px;
}

.editor {
  min-height: 300px;
  background-color: transparent;
  color: #fff;
  border: none;
  font-size: 15px;
  line-height: 23px;
  white-space: pre-wrap;
  overflow-wrap: break-word;
}

.editor:focus {
  outline: none;
}

.editor p {
  margin: 0;
  padding: 0;
}

.editor:empty::before {
  content: "Введите текст...";
  color: #888;
  font-style: italic;
}
</style>
