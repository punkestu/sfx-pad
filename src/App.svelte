<script>
  let audios = new Array(16).fill(null);
  let isSetup = false;
  let filesInput;
  let files;
  let selectedId = null;
  let filePopover;

  $: handleFiles(files);

  function handleFiles(_files) {
    if (_files) {
      if (_files.length === 1 && selectedId) {
        audios[selectedId] = _files[0];
        selectedId = null;
        _files = null;
        filesInput.value = "";
      } else if (_files.length > 0) {
        for (let i = 0; i < _files.length; i++) {
          audios[i] = _files[i];
        }
      }
      filePopover.hidePopover();
    }
  }

  function paneClicked(id) {
    if (isSetup) {
      selectedId = id;
    } else {
      if (audios[id]) {
        const audio = new Audio(URL.createObjectURL(audios[id]));
        audio.play();
        audio.onended = () => {
          audio.src = "";
        };
      }
    }
  }

  window.addEventListener("keydown", (e) => {
    if (e.key === "1") {
      paneClicked(0);
    } else if (e.key === "2") {
      paneClicked(1);
    } else if (e.key === "3") {
      paneClicked(2);
    } else if (e.key === "4") {
      paneClicked(3);
    } else if (e.key === "5") {
      paneClicked(4);
    } else if (e.key === "6") {
      paneClicked(5);
    } else if (e.key === "7") {
      paneClicked(6);
    } else if (e.key === "8") {
      paneClicked(7);
    } else if (e.key === "9") {
      paneClicked(8);
    } else if (e.key === "0") {
      paneClicked(9);
    } else if (e.key === "q") {
      paneClicked(10);
    } else if (e.key === "w") {
      paneClicked(11);
    } else if (e.key === "e") {
      paneClicked(12);
    } else if (e.key === "r") {
      paneClicked(13);
    } else if (e.key === "t") {
      paneClicked(14);
    } else if (e.key === "y") {
      paneClicked(15);
    }
  });
</script>

<main class="p-4 flex flex-col">
  <div id="file-popover" popover="auto" bind:this={filePopover}>
    <input type="file" bind:files bind:this={filesInput} multiple />
  </div>
  <label for="isSetup" class="self-center mb-4">
    <input type="checkbox" id="isSetup" bind:checked={isSetup} />
    Load audio
  </label>
  <div id="audio-board" class="grid grid-cols-8 gap-4">
    {#each audios as audio, i}
      <button
        class="{audio
          ? 'bg-gray-300 hover:bg-slate-200 text-gray-800'
          : 'bg-gray-700 hover:bg-gray-600 text-white'} aspect-square text-2xl font-semibold text-wrap break-words rounded-md p-2"
        popovertarget={isSetup ? "file-popover" : ""}
        on:click={() => paneClicked(i)}
      >
        {i + 1 > 9 ? ["q", "w", "e", "r", "t", "y", "u"][i - 9] : i + 1} <br />
        {audio ? audios[i].name : ""}</button
      >
    {/each}
  </div>
</main>
