#Nov-1
export default class App extends Component {
    state={
        h:0,m:0,s:0,
        cur:0,
        tswp:['12:00','16:00','20:00']
    }

    componentDidMount(){
        this.timerval()
    }

    getcur=()=>{
        let {tswp,cur}=this.state
        let now = new Date();
        let year = now.getFullYear();       //年
        let month = now.getMonth() + 1;     //月
        let day = now.getDate();            //日
        // console.log(new Date().getTime(),year,month,day,Date.parse(new Date(`${year}-${month}-${day} ${tswp[cur]}`)));
        const over=Date.parse(new Date(`${year}-${month}-${day} ${tswp[cur]}`))
        this.endtime(over)
    }
 //卸载组件取消倒计时
    componentWillUnmount(){
         clearInterval(this.timer);
       }
    endtime=(end)=>{
        let curtime=new Date().getTime()
        let dtime=end-curtime
            // let day = Math.floor((dtime / 1000 / 3600) / 24);
            let hour = Math.floor((dtime / 1000 / 3600) % 24);
            let minute = Math.floor((dtime / 1000 / 60) % 60);
            let second = Math.floor(dtime / 1000 % 60);
            this.setState({h:hour,m:minute,s:second})
    }
    timerval=()=>{
        this.timer=setInterval(()=>{
            this.getcur()
        },1000)
    }

    tab=(index)=>{
        this.setState({cur:index})
        this.timerval()
    }

    render() {
        let {h,m,s,cur,tswp}=this.state
        return (
            <div className="app">
                <p>
                    <span>据本场结束：{h}时{m}分{s}秒</span>
                    {
                        tswp.map((item,index)=>(
                            <span 
                            onClick={()=>this.tab(index)} 
                            key={index}>{item}</span>
                        ))
                    }
                </p>
                
            </div>
        )
    }
}
