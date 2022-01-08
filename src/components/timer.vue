<template>
  <div class="wrapper">
    <div ref="ring" class="ring">
      <svg width="518" height="518" viewBox="0 0 518 518">
        <circle stroke-width="9px" x="0" y="y" cx="259" cy="259" r="254" />
      </svg>
    </div>
    <div class="timer">
      <div class="time">
        <div class="minutes">
          <input ref="mins" type="text" value="15" disabled />
        </div>
        <div class="colon">:</div>
        <div class="seconds">
          <input ref="seconds" type="text" value="00" disabled />
        </div>
      </div>
      <button class="start" ref="work" v-on:click="Work">start</button>
      <button class="settings" ref="settings" v-on:click="Settings">
        <img src="../assets/images/gear.svg" alt="Settings" />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Timer",
  data: function () {
    return {
      nIntervId: null,
      setting: true,
      items: [
        {
          name: "Start",
          class: "ring",
          selected: 1,
        },
        {
          name: "Pause",
          class: "ring ending",
          selected: 0,
        },
      ],
    };
  },
  methods: {
    Work() {
      /*
        Inicia o para el contador, dependiendo del estado
        Cambia el texto del estado indicando si esta activo o pausado
        si esta activo debe cambiarse el color del contador a rojo
      */
      let old_item = this.items.find((element) => element.selected > 0);
      let new_item = this.items.find((element) => element.selected == 0);

      this.$refs.ring.classList.value = new_item.class;
      this.$refs.work.innerText = new_item.name;

      old_item.selected = 0;
      new_item.selected = 1;

      if (new_item?.name != "Start") {
        this.$refs.mins.disabled =
          this.$refs.seconds.disabled =
          this.setting =
            true;
      }

		if (old_item?.name == "Start") {
			this.nIntervId = setInterval(function (timer) {
				let minutes = Number(timer.$refs.mins.value);
				let seconds = Number(timer.$refs.seconds.value) - 1;
				if (minutes > 0 || minutes == 0) {
					if (seconds < 0){
						if (minutes > 0) minutes--; 
						seconds = 59;
					}
					
				}
				
				timer.$refs.mins.value = (minutes < 10 ? "0" + minutes : minutes);
				timer.$refs.seconds.value = (seconds < 10 ? "0" + seconds : seconds);

				if (minutes == 0 && seconds == 0) {
					timer.Work();
					return false;
				}
			}, 1000, this);
		} 
		else clearInterval(this.nIntervId);
    },
    Settings() {
      /*
        Permite editar los inputs de la hora y minutos de cuenta atras
      */
      if (this.items.find((element) => element.selected > 0)?.name == "Start")
        this.$refs.mins.disabled =
          this.$refs.seconds.disabled =
          this.setting =
            !this.setting;
    },
  },
};
</script>