marque -> [without display flex] -> give to parent 
    white-space: nowrap;
    overflow-x: auto;
    overflow-y: hidden;
 - give to child   
    display: inline-block;

    - with this mwthod we can build scrollable and animation
<!-- --------------------------------------------------------------------- -->
display: inline-block;-> content ki width content ke equal kr deta hai
         aur unke beech me 4px ka gap bna deta h.to child ko "margin-right:16px" dedo to gap total 20px ho jayega aur "calc" me gap ko handle kro  
    - In :- 
    @keyframes scroll2{
    from{
        transform: translateX(0%);
    }
    to{
        transform: translateX(calc(-100%, -20px));-> 
    }
}
<!-- --------------------------------------------------------------------- -->
