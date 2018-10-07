<template>
    <section>
        <h1>Click on 'Count' button to count your clicks</h1>
        <button v-stream:click="count$">Count clicks</button>
        <button @click="clearCounter">Clear counter</button>
        <p>{{result$}}</p>
    </section>
</template>

<script>
import {
  filter,
  bufferWhen,
  debounceTime,
  map,
  startWith,
} from 'rxjs/operators';
export default {
  domStreams: ['count$'],
  subscriptions() {
    return {
      result$: this.count$.pipe(
        filter(event => !!event),
        bufferWhen(() => this.count$.pipe(debounceTime(400))),
        map(clicks => clicks.length),
        startWith(0),
      ),
    };
  },
  methods: {
    clearCounter() {
      this.count$.next(null);
    },
  },
};
</script>

<style>
button {
  padding: 10px;
  font-size: 14px;
  margin-right: 10px;
  border-radius: 4px;
  outline: none;
}
</style>