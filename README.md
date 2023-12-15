# How to create a logger to monitor your Redux in Reactotron:
A simpler and faster way to monitor `redux-toolkit` actions. This way, no other libraries or extra configurations are needed; I just modified the `dispatch` of the `store` to also generate logs in `reactotron`.

## Prerequisites
Before you start, you'll need to have the following libraries installed in your project:
- [redux-toolkit](https://redux-toolkit.js.org/)
- [reactotron](https://github.com/infinitered/reactotron)

## Code
Within your `store`, simply configure it as follows:
```tsx
import {configureStore} from '@reduxjs/toolkit';
import auth from './auth'; // your redux slice here
import reactotron from 'reactotron-react-native';

export const store = configureStore({
  reducer: {
    auth,
  },
});

const next = store.dispatch;
store.dispatch = function dispatchAndLog(action: any) {
  const PREV_STATE = store.getState();
  let result = next(action);
  const NEXT_STATE = store.getState();
  reactotron.stateActionComplete(action.type, {
    PREV_STATE,
    NEXT_STATE,
    PAYLOAD: action.payload,
  });
  return result;
};
```
## Result
<p align="center">
  <img src="https://github.com/uesleisuptitz/reactotron-redux-logger/blob/master/Result.png" alt="Result of logs" />
</p>
