# std::inserter_iterator<Container>

std::insert_iterator 是一个 LegacyOutputIterator，它将元素插入到为其构造的容器中，插入位置由提供的迭代器指向。每当迭代器（无论是解引用还是不解引用）被赋值时，都会调用容器的 insert() 成员函数。递增 std::insert_iterator 是一个空操作。

他能够作为一个输出迭代器用于(stl算法中指定输出的位置，能保证迭代器不失效且自身位置不变的效果(固定指向一个元素))

真正使用的时候有三个辅助函数
* inserter(container,iterator) 返回 inserter_iterator
* back_inserter(container) 返回 back_inserter_iterator
* front_inserter(container) 返回 front_inserter_interator