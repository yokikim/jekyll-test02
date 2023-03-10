---
layout: page
title: upcall
parent: 스케줄러-액티베이션

---

212p



...커널은 응용에 [가상 처리기](경량-프로세스.md) (LWP)의 집합을 제공하고 응용은 사용자 스레드를 가용한 가상 처리기로 스케줄 한다.

게다가 커널은 응용에게 특정 이벤트에 대해 알려줘야 한다.

이 [프로시저](프로시저.md)를 **upcall** 이라고 부른다,

Upcall은 스레드 라이브러리의 upcall 처리기에 의해 처리되고, **upcall 처리기** 는 [가상 처리기](경량-프로세스.md)상에서 실행되어야 한다.

...

Upcall을 일으키는 한 이벤트는 응용 스레드가 [봉쇄](봉쇄형.md)하려고 할 때 발생한다.

이 시나리오에서 커널은 스레드가 봉쇄하려고 한다는 사실과 그 스레드의 식별자를 알려 부는 upcall을 한다.

그런 후에 커널은 새로운 가상 처리기를 응용에 할당한다,

응용은 이 새로운 가상 처리기상에서 upcall 처리기를 수행하고 이 upcall 처리기는 봉쇄 스레드의 상태를 저장하고 이 스레드가 실행 중이던 가상 처리기를 반환한다.

그리고 upcall 처리기는 새로운 가상 처리기에서 실행 가능한 다른 스레드를 스케줄 한다.

봉쇄 스레드가 기다리던 이벤트가 발생하면 커널은 이전에 봉쇄되었던 스레드가 이제 실행할 수 있다는 사실을 알려주는 또 다른 upcall을 [스레드 라이브러리](스레드-라이브러리.md)에 한다.

이 이벤트를 처리하는 Upcall 처리기도 가상 처리기가 필요하고 커널은 새로운 가상 처리기를 할당하거나 사용자 스레드 하나를 선점하여 그 처리기에서 이 upcall 처리기를 실행한다.

봉쇄가 풀린 스레드를 실행 가능 상태로 표시한 후에 응용은 가용한 가상 처리기상에서 다른 실행 가능한 스레드를 실행한다.