# RxSwift to Combine Cheatsheet
This is a Cheatsheet for [RxSwift](https://github.com/ReactiveX/RxSwift) developers interested in Apple's new [Combine](https://developer.apple.com/documentation/combine) framework.

It's based on the following blog post: [https://medium.com/gett-engineering/rxswift-to-apples-combine-cheat-sheet-e9ce32b14c5b](https://medium.com/gett-engineering/rxswift-to-apples-combine-cheat-sheet-e9ce32b14c5b)

## [Basics](Data/basics.csv)

|                       | RxSwift                          | Combine                                    |
|-----------------------|----------------------------------|--------------------------------------------|
| Deployment Target     | iOS 8.0+                         | iOS 13.0+                                  |
| Platforms supported   | iOS, macOS, tvOS, watchOS, Linux | iOS, macOS, tvOS, watchOS, UIKit for Mac ¹ |
| Spec                  | Reactive Extensions (ReactiveX)  | Reactive Streams (+ adjustments)           |
| Framework Consumption | Third-party                      | First-party (built-in)                     |
| Maintained by         | Open-Source / Community          | Apple                                      |
| UI Bindings           | RxCocoa                          | SwiftUI ²                                  |


## [Core Components](Data/core_components.csv)

| RxSwift                   | Combine                  | Notes                                                                                                                                                           |
|---------------------------|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AnyObserver               | AnySubscriber            |                                                                                                                                                                 |
| BehaviorRelay             | ❌                        | Simple wrapper around BehaviorSubject, could be easily recreated in Combine                                                                                     |
| BehaviorSubject           | CurrentValueSubject      | This seems to be the type that holds @State under the hood                                                                                                      |
| Completable               | ❌                        |                                                                                                                                                                 |
| CompositeDisposable       | ❌                        |                                                                                                                                                                 |
| ConnectableObservableType | ConnectablePublisher     |                                                                                                                                                                 |
| Disposable                | Cancellable              |                                                                                                                                                                 |
| DisposeBag                | ❌                        |                                                                                                                                                                 |
| Driver                    | BindableObject (SwiftUI) | Both guarantee no failure, but Driver guarantees delivery on Main Thread. In Combine, SwiftUI recreates the entire view hierarachy on the Main Thread, instead. |
| Maybe                     | ❌                        |                                                                                                                                                                 |
| Observable                | Publisher                |                                                                                                                                                                 |
| Observer                  | Subscriber               |                                                                                                                                                                 |
| PublishRelay              | ❌                        | Simple wrapper around PublishSubject, could be easily recreated in Combine                                                                                      |
| PublishSubject            | PassthroughSubject       |                                                                                                                                                                 |
| ReplaySubject             | ❌                        |                                                                                                                                                                 |
| ScheduledDisposable       | ❌                        |                                                                                                                                                                 |
| SchedulerType             | Scheduler                |                                                                                                                                                                 |
| SerialDisposable          | ❌                        |                                                                                                                                                                 |
| Signal                    | ❌                        |                                                                                                                                                                 |
| Single                    | Future                   | Single behaves differently than Future. Seems the latter has no auto-disposal when finished                                                                     |
| SubjectType               | Subject                  |                                                                                                                                                                 |
| TestScheduler             | ❌                        | There doesn't seem to be an existing testing scheduler for Combine code                                                                                         |


## [Operators](Data/operators.csv)

| RxSwift              | Combine                                      | Notes                                                                                                    |
|----------------------|----------------------------------------------|----------------------------------------------------------------------------------------------------------|
| amb()                | ❌                                            |                                                                                                          |
| asObservable()       | eraseToAnyPublisher()                        |                                                                                                          |
| asObserver()         | eraseToAnySubject()                          |                                                                                                          |
| bind(to:)            | `assign(to:on:)`                             | Assign uses a KeyPath which is really nice and useful. RxSwift needs a Binder / ObserverType to bind to. |
| buffer               | buffer                                       |                                                                                                          |
| catchError           | catch                                        |                                                                                                          |
| catchErrorJustReturn | replaceError(with:)                          |                                                                                                          |
| combineLatest        | combineLatest, tryCombineLatest              |                                                                                                          |
| compactMap           | compactMap, tryCompactMap                    |                                                                                                          |
| concat               | append, prepend                              |                                                                                                          |
| concatMap            | ❌                                            |                                                                                                          |
| create               | AnyPublisher                                 | AnyPublisher has an initializer with an anonymous closure, similar to Observable.create                  |
| debounce             | debounce                                     |                                                                                                          |
| debug                | print                                        |                                                                                                          |
| deferred             | Publishers.Deferred                          |                                                                                                          |
| delay                | delay                                        |                                                                                                          |
| delaySubscription    | ❌                                            |                                                                                                          |
| dematerialize        | ❌                                            |                                                                                                          |
| distinctUntilChanged | removeDuplicates, tryRemoveDuplicates        |                                                                                                          |
| do                   | handleEvents                                 |                                                                                                          |
| elementAt            | output(at:)                                  |                                                                                                          |
| empty                | Publishers.Empty(completeImmediately: true)  |                                                                                                          |
| enumerated           | ❌                                            |                                                                                                          |
| error                | Publishers.Once                              | Publishers.Once has an initializer that takes an Error                                                   |
| filter               | filter, tryFilter                            |                                                                                                          |
| first                | first, tryFirst                              |                                                                                                          |
| flatMap              | flatMap                                      |                                                                                                          |
| flatMapFirst         | ❌                                            |                                                                                                          |
| flatMapLatest        | switchToLatest                               |                                                                                                          |
| from                 | ❌                                            |                                                                                                          |
| groupBy              | ❌                                            |                                                                                                          |
| ifEmpty(default:)    | replaceEmpty(with:)                          |                                                                                                          |
| ifEmpty(switchTo:)   | ❌                                            | Could be achieved with composition - replaceEmpty(with: publisher).switchToLatest()                      |
| ignoreElements       | ignoreOutput                                 |                                                                                                          |
| interval             | ❌                                            |                                                                                                          |
| just                 | Publishers.Just                              |                                                                                                          |
| map                  | map, tryMap                                  |                                                                                                          |
| materialize          | ❌                                            |                                                                                                          |
| merge                | merge, tryMerge                              |                                                                                                          |
| multicast            | multicast                                    |                                                                                                          |
| never                | Publishers.Empty(completeImmediately: false) |                                                                                                          |
| observeOn            | receive(on:)                                 |                                                                                                          |
| of                   | ❌                                            |                                                                                                          |
| publish              | makeConnectable                              |                                                                                                          |
| range                | ❌                                            |                                                                                                          |
| reduce               | reduce, tryReduce                            |                                                                                                          |
| refCount             | autoconnect                                  |                                                                                                          |
| repeatElement        | ❌                                            |                                                                                                          |
| retry, retry(3)      | retry, retry(3)                              |                                                                                                          |
| retryWhen            | ❌                                            |                                                                                                          |
| sample               | ❌                                            |                                                                                                          |
| scan                 | scan, tryScan                                |                                                                                                          |
| share                | share                                        | There’s no replay or scope in Combine. Could be “faked” with multicast.                                  |
| skip(3)              | dropFirst(3)                                 |                                                                                                          |
| skipUntil            | drop(untilOutputFrom:)                       |                                                                                                          |
| skipWhile            | drop(while:), tryDrop(while:)                |                                                                                                          |
| startWith            | prepend                                      |                                                                                                          |
| subscribe            | sink                                         |                                                                                                          |
| subscribeOn          | subscribe(on:)                               | RxSwift uses Schedulers. Combine uses RunLoop, DispatchQueue, and OperationQueue.                        |
| takeLast             | last                                         |                                                                                                          |
| takeUntil            | prefix(untilOutputFrom:)                     |                                                                                                          |
| throttle             | throttle                                     |                                                                                                          |
| timeout              | timeout                                      |                                                                                                          |
| timer                | Timer.publish                                |                                                                                                          |
| toArray()            | collect()                                    |                                                                                                          |
| window               | collect(Publishers.TimeGroupingStrategy)     | Combine has a TimeGroupingStrategy.byTimeOrCount that could be used as a window.                         |
| withLatestFrom       | ❌                                            |                                                                                                          |
| zip                  | zip                                          |                                                                                                          |


# Contributing
Add any data/operators to the appropriate CSV files in the **Data** folder, run `bundle install` and `generate.rb`.

Finally, commit the changes and submit a Pull Request.