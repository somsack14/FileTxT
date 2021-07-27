
class AllCategory extends StatefulWidget {
  final count;

  AllCategory({Key key, @required this.count})
      : super(key: key);

  @override
  _AllCategoryState createState() => _AllCategoryState();
}

class _AllCategoryState extends State<AllCategory> {
  
  ///create list Model
  List<Data> productArrays = [];

  ///loop data to array
  Future<void> setListItems() async {
    Provider.of<ApplyLoanProvider>(context, listen: false)
        .getAllProductList
        .data
        .forEach((element) {
      if (element.categoryId == widget.count) productArrays.add(element);
    });
  }
  

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    setListItems();
  }

  @override
  Widget build(BuildContext context) {
    final ApplyLoanProvider applyLoanProvider =
        Provider.of<ApplyLoanProvider>(context);
    SizeConfig().init(context);
    return Scaffold(
      appBar: AppBar(
        title: MyText().textHeadSize25(
            'ສິນຄ້າ', MyStyle().whiteColor, SizeConfig.screenWidth),
        backgroundColor: MyStyle().primaryColor,
      ),
      body: Container(
        child: GridView.builder(
          shrinkWrap: true,
          physics: new NeverScrollableScrollPhysics(),
          gridDelegate: SliverGridDelegateWithMaxCrossAxisExtent(
            maxCrossAxisExtent: 200,
            crossAxisSpacing: 14,
            mainAxisSpacing: 5,
            childAspectRatio:
                (SizeConfig.screenWidth / SizeConfig.screenHeight / 0.69),
          ),
          itemCount: productArrays.length,
          itemBuilder: (context, index) {
            return Card(
              color: Colors.white,
              clipBehavior: Clip.antiAlias,
              shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.circular(20.0),
              ),
              child: InkWell(
                borderRadius: BorderRadius.circular(20),
                onTap: () {
                  // if(widget.count == applyLoanProvider.getAllProductList.data[index].categoryId){
                  //   return;
                  // }
                  print('index all product : $index');
                  Navigator.push(
                      context,
                      MaterialPageRoute(
                          builder: (context) => ProductDetail(
                                count: index,
                              )));
                },
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    ClipRRect(
                      borderRadius: BorderRadius.circular(20),
                      child: Container(
                        clipBehavior: Clip.antiAlias,
                        height: SizeConfig.screenHeight * 0.2,
                        decoration: BoxDecoration(
                          borderRadius: BorderRadius.circular(20),
                        ),
                        child: Stack(
                          children: [
                            Positioned(
                              child: Container(
                                child: Ink.image(
                                  image: NetworkImage(
                                      "${MyDomain().domain + productArrays[index].productImages[0].toString()}"),
                                  fit: BoxFit.fitWidth,
                                ),
                              ),
                            ),
                          ],
                        ),
                      ),
                    ),
                    Expanded(
                      child: Padding(
                        padding: EdgeInsets.symmetric(
                            horizontal: getProportionateScreenWidth(5)),
                        child: Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            MyText().textHeadSizeCustomOverflow(
                                " ${productArrays[index].productName}"
                                    .toString(),
                                Colors.black,
                                FontWeight.normal,
                                SizeConfig.screenWidth * 0.04),
                            MyText().textHeadSizeCustomOverflow(
                                productArrays[index]
                                    .productDescription
                                    .toString(),
                                Colors.grey[700],
                                FontWeight.normal,
                                SizeConfig.screenWidth * 0.025),
                            MyText().textHeadSizeCustom(
                                currencyFormat.format(int.tryParse(
                                    applyLoanProvider.getAllProductList
                                        .data[index].variants[0].price
                                        .toString())),
                                Colors.orange,
                                FontWeight.bold,
                                SizeConfig.screenWidth * 0.045)
                          ],
                        ),
                      ),
                    ),
                    ClipPath(
                      clipper: ClipperNameStoreProduct(),
                      child: Container(
                        height: SizeConfig.screenHeight * 0.03,
                        width: SizeConfig.screenWidth * 0.35,
                        padding: EdgeInsets.only(
                            left: getProportionateScreenWidth(15)),
                        color: MyStyle().yellowColor.withOpacity(0.9),
                        child: Align(
                          alignment: Alignment.centerLeft,
                          child: MyText().textHeadSizeCustomOverflow(
                              applyLoanProvider
                                  .getAllProductList.data[index].storeName
                                  .toString(),
                              MyStyle().primaryColor,
                              FontWeight.normal,
                              SizeConfig.screenWidth * 0.04),
                        ),
                      ),
                    ),
                  ],
                ),
              ),
            );
          },
        ),
      ),
    );
  }
}
