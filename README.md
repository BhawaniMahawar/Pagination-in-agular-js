# Pagination-in-agular-js
very simple way of making pagination in angular js


 function getArea(PageIndex, PageSize) {

            vm.areaArr = [];
            return DataContext.getArea(PageIndex, PageSize).then(function (data) {
                var d = data[0].data;

                vm.areaArr = d;

                if (PageIndex = 1) {
                    if (vm.areaArr.length > 0) {
                        vm.totalRec = Math.ceil(vm.areaArr[0].recCount / PageSize);

                        var maxLimit = vm.totalRec;
                        if (vm.totalRec > 10)
                            maxLimit = 10;
                        for (var i = 0; i < maxLimit ; i++) {
                            vm.pageingList.push(i + 1);
                        }
                    }
                }

                return data;
            });
        }



vm.CurrentPage = function (curPageNum, totalRec) {
           
            console.log(totalRec);
            console.log(curPageNum);
         

            TotalPg = vm.totalRec;
      
            $(".PageNum").css("color", "");
            $("#PageNum" + curPageNum).css("color", "Red");
            vm.pageingList = [];
            var minLimit = 0;
            var maxLimit = TotalPg;
            if (TotalPg <= 10) {
                minLimit = 0;
                maxLimit = TotalPg;
            }
            else {
               maxLimit = curPageNum;
               
                if (curPageNum <= 5) {
                    maxLimit = 10;
                    minLimit = 0;
                }
                else {
                    if ((curPageNum + 5) <= TotalPg) {
                        maxLimit = curPageNum + 5;
                        minLimit = curPageNum - 5;
                    }
                    else {
                        maxLimit = maxLimit + (TotalPg - curPageNum);
                       // minLimit = minLimit + (totalRec - curPageNum);
                       minLimit = maxLimit-10;
                    }
                }
            }
            if (minLimit < 1)
                minLimit = 0;
            else
                minLimit = minLimit;
            for (var i = minLimit; i < maxLimit; i++) {
                vm.pageingList.push(i + 1);
            }

            common.dataFetchBegin();
            var promise = [getArea(curPageNum, 5)];
            common.activateController(promise, controllerId);
        }
