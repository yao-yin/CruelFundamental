## constructor和desctructor能virtual吗

构造函数不能是虚函数但是析构函数可以是虚函数。

对于构造函数而言，由于虚函数对应一张虚函数表，虚函数表存储在对象的的内存空间中，但是对象需要通过构造函数实例化之后才拥有空间，因此存在矛盾。另外构造函数本身作用是初始化且对象生命周期内只会执行一次，因此无需成为虚函数。

对于析构函数而言，当类存在其它派生类时，为保证父类指针调用子类函数，需要将该函数实现为虚函数，为了避免内存泄露，因此当类存在其它派生类时，需要将父类析构函数实现为虚函数。